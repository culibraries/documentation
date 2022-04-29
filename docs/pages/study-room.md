# Room Reservation Tablets

## Overview

We were tasked to deploy a study room reservation system at the Engineering, Math and Physics Library (Gemmill) to enable patrons to check room availability and make reservations at each designated study room location. This project is intended to enhance the user experience of reserving study rooms by providing the capability at the point of need.

This project was also intended to serve a prototype to inform a more comprehensive and common solution to meet the needs of all branches of University Libraries.

The Libraries also produced a [video about the project](https://www.youtube.com/watch?v=PQ_0srwgagI)

## System Specification

### Application Features

* Check room (current or other rooms in the same locations) availability for current and future dates.
* Reserve room using BuffOne card or email.
* Send a reservation confirmation and cancel a reservation via email (Handled by LibCal)

### Hardware Specification

* Tablet from [Mimo Monitors Company](https://www.mimomonitors.com)
  * Model is `Mimo Adapt-IQV 10.1" Digital Signage Tablet Android 6.0 - RK3288 Processor MCT-10HPQ`
  * [Exact model](https://www.mimomonitors.com/collections/10-1-tablets/products/mimo-adapt-iqv-10-1-digital-signage-tablet?variant=6403183149088)
  * We do not need PoE (Power over Ethernet)
  * Support via techsupport@mimomonitors.com
* Magnetic Swipe Card : IDTech Company
  * Suggested by the Buff OneCard Office
  * Contact the Buff OneCard office for support

## Starting a tablet for the first time

### Preparation

Login to [Libcal](https://colorado.libcal.com/admin/)

* Get `Location ID` from LibCal.
  * Admin => Equipment & Space => Looking at the column `ID` for `location ID`
* Get `Hours View ID` from LibCal.
  * Admin => Hours => Widgets => JSON-LD Data => Select `Library` => Click `Generate Code/Previews` => Looking at the `lid number` in `Embed Code` section
* Get `Space ID` from LibCal.
  * Admin => Equipment & Space => In column `Spaces` => Click the space number (Ex: 3 spaces, 5 spaces) =>  Looking at the column `ID` for `space ID`

### Software Setup

Only users who have administrative permission is able to follow the steps below to setup the software for the application.

1. Login to room booking admin
    * Production <https://libapps.colorado.edu/room-booking-admin>
    * Test <https://test-libapps.colorado.edu/room-booking-admin>
1. Click “New Device”.
1. Click “Generate” to generate new id for the device.
1. Fill out all of the information including:
    * Name
    * Note (optional)
    * `Location ID`
    * `Hours View ID`
    * `Space ID`
1. Click “Submit”

```{warning}
Note : The users are not able to Activate/Deactivate/Delete the device at their computer, they must activate it at the device where it is to be installed.
```

### Wireless Setup

1. Connect the tablet to `UCB Wireless` WiFi. The tablet should be able to connect to the network but not authenticate onto the WiFi.
1. Find the device's WiFi MAC address
    1. Unlock the tablet
       1. Touch/Hold the top-right corner of the tablet for 10+ seconds then a password dialog will pop-up
       1. Enter the pin (In Keepass: `Study Room Application/Hardware Tablet Pin`).
    1. Use the `Home/House` icon in the Mimo app to get to the "Desktop"
    1. Slide open the top menu from the upper right corner and click the `gear` icon to access the settings
    1. Scroll to the bottom and click `About tablet`
    1. Click `Status`
    1. Find `Wi-Fi MAC address`
1. Leave the tablet connected to the network
1. Open a ServiceNow ticket with Dedicated Desktop Support ticket (DDS)

    ```txt
    Please add the following MAC addresses to SafeConnect with the user `libnotify@colorado.edu`. These are for android tablets that we are deploying to support an in-place room reservation system in the Libraries.

    ma:c1
    ma:c2
    ma:c3

    Thank you!
    CTA
    ```

1. Wait 20 minutes after DDS adds the devices to SafeConnect for the configuration to propagate.
1. "Forget" the existing "UCB Wireless" configuration and reselect "UCB Wireless".

The device should connect and you should be able to access webpages via a browser.

### Device Setup

In the Android Settings menus:

1. Time/Date Setup
   1. Settings > Date & Time
   1. Turn off “Automatic time zone”
   1. “Select time zone” to “Denver GMT-07:00”
   1. Turn on “Automatic time zone”
1. Text-correction Setup
   1. Settings > Language & Input > Android Keyboard (AOSP) > Text correction
   1. Switch all off except “Block offensive words”

### Application Setup

1. Unlock the tablet
    1. Touch/Hold the top-right corner of the tablet for 10+ seconds then a password dialog will pop-up
    1. Enter the pin (In Keepass: `Study Room Application/Hardware Tablet Pin`).
1. Open "MLock" application
1. Press “Cookie” then turn on Cookie State
    * Pink is ON, Gray is OFF
    * Press “< Cookie Setting” to go back.
1. Press “Default App” and turn on “Auto Start”
    * Pink is ON, Gray is OFF
    * Press “< Default App” to go back
1. Press “Playback Setting” > “Web URL” and change the url
    * Production <https://libapps.colorado.edu/room-booking-admin>
    * Test <https://test-libapps.colorado.edu/room-booking-admin>
    * Press the "go back" icon on the top-left
1. Login using the admin user credential in (Software setup) section above, then activate the device that you want to set up in the list by turn on the toggle button.
    * Pink is Activated, Gray is Deactivated
1. Plug Magnetic Swipe Card device to the USB port

```{warning}
In production, only undergraduate students can reserve the rooms. Faculty, Staff, or graduate student will receive an error message. In Test, all staff can reserve the room.
```

## Code Repositories

* [Room Booking application (Main Interface)](https://github.com/culibraries/room-booking)
* [Room Booking admin (Admin Interface](https://github.com/culibraries/room-booking-admin)
* [API (Cybercomm)](https://github.com/culibraries/room-booking-django-app)

### Admin User

In order to access to Room Booking admin application, you need to create a local user in Cybercomm with a password and add group `study-room-admin` to that user as the permission. Store the username and password in a safe place. You will need to use that to set up the tablet.

Basically, The tablet will interact with API (Cybercomm) using the Token from this admin user, which is stored in localStorage in each device. In Application Setup -> Step 5 (in the section above), The reason you need to go to room-booking-admin is to get the Token and store it in each tablet for future API call.

### Room Booking Admin

This application is to manage information in each tablet. You can add/delete/modify the information of each room which corresponding to each tablet in front of the room. However, you are not able to activate unless you access to this application in a tablet.

Do not forget to Generate Unique ID for each tablet.

### Room Booking

#### Local Development

1. Create a testing room in Libcal and get all of information including: location_id, space_id, hours_view_id.
1. Get Token string from admin user above, and libcal_token (It will be expired after 60 minutes.)
1. Store all of these variable in LocalStorage of the web browser.
1. Go to auth-guard.service.ts and remove line 26,27. (Don't forget to bring it back when deploy to TEST or PRODUCTION.

You can plugging the Magnetic Swipe Card into your computer via USB port and use it as a testing device.

### API (Cybercom)

* Get libcal_token from Libcal using LIBCAL_CLIENT_ID and LIBCAL_CLIENT_SECRET (in Libops).
* Get information after swipe action using sierra api.

### Logs

All the log will be uploaded to S3 at cubl-log/room-booking as CSV format around midnight everyday.
Terms explanation in the log:

* refresh libcal_token: Since the token only good for 60 minutes, so it will need to recreated when it is expired.
* app starting...: this happen when the application refresh the page.
* card - PType - Room (email and time slots): when these 3 messages go together. It means someone successfully book the room.
* the library is closed: It means the tablet is not display for booking anymore since the library is closed.
Please looking the github code for more log information.

## Support Documentation For Circ Desk Staff

```{note}
Bring a USB keyboard with you to debug. Disconnect the USB card reader in order to connect the keyboard.
```

1. What should I do if the message “SYSTEM ERROR” appears?
    * If a student reports to Circ Desk about the error above:
        1. Go to the tablet and press “Press here to reload”. If the problem isn’t fixed, continue.
        1. Reboot the tablet.
    * If the error persists, manually reserve the room for that student via libcal, and submit a ticket to University Libraries - Core Tech & Apps.
1. What should I do if there is an internet connection issue?
    * If internet connection is active, submit a ticket to University Libraries - Core Tech & Apps
    * If there is no internet connectivity in the building, contact OIT for a network connection issue.
1. How can I reboot the tablet?
    * There is a power switch underneath the tablet on the left side.
    * You can turn the tablet off and on to reboot it.
1. What should I do if the screen turns black?
      * Touch the screen to see if its back on.
      * Check to see if there is any power outage.
      * Reboot the tablet.
      * If the issue is not resolved, submit a ticket to University Libraries - Core Tech & Apps.
1. Why student successfully reserve a room through Libcal but it is not showing on the tablet?
    * It can take 45 seconds to 60 seconds for the tablet pull information from Libcal. If the student has an email confirmation from Libcal it should update.
1. For any other issue, begin with rebooting the tablet.
    * As a workaround, manually reserve the room for the student via Libcal
    * Submit a ticket to University Libraries - Core Tech & Apps.
