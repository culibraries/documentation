# Study Room 

## Overview

https://www.youtube.com/watch?v=PQ_0srwgagI

This project to deploy a study room reservation system at the Engineering, Math and Physics Library (Gemmill) that will enable patrons to check room availability and make reservations at each designated study room location. This project is intended to enhance the user experience of reserving study rooms by providing the capability at the point of need.

This project is also intended to serve a prototype to inform a more comprehensive and common solution to meet the needs of all branches of University Libraries.

## System Specification

### Application Features
* Check room (current or other rooms in the same locations) availability for current and future dates.
* Reserve room using BuffOne card or email.
* Send a reservation confirmation and cancel a reservation via email (Handled by LibCal)

### Hardware Specification 
* Tablet : Mimo Monitors Company: https://www.mimomonitors.com/
      Mimo Adapt-IQV 10.1" Digital Signage Tablet Android 6.0 - RK3288 Processor MCT-10HPQ
* Magnetic Swipe Card : IDTech Company - We got this suggestion from Buff OneCard Office.

## Starting the monitor for the first time 
### Preparation 

Login to Libcal : https://colorado.libcal.com/admin/
* Get “location ID” from LibCal.
Admin > Equipment & Space > Looking at the column “ID” for location ID
* Get “hours view ID” from LibCal.
Admin > Hours > Widgets > JSON-LD Data > Select “Library” > Click “Generate Code/Previews” > Looking at the lid number in “Embed Code” section 
* Get “space ID” from LibCal.
Admin > Equipment & Space > In column “Spaces” > Click the space number (Ex: 3 spaces, 5 spaces…) >  Looking at the column “ID” for space ID

### Software Setup
Only users who have administrative permission is able to follow the steps below to setup the software for the application.
* Step 1 : Login to room booking admin : 
https://libapps.colorado.edu/room-booking-admin (PRODUCTION) https://test-libapps.colorado.edu/room-booking-admin (TEST)
* Step 2 : Click “New Device”.
* Step 3 : Click “Generate” to generate new id for the device.
* Step 4 : Fill out all of the information including : Name, Note (optional), Location ID, Space ID, Hours View ID (from 1a, b, c).
* Step 5 : Click “Submit”.
Note : The users are not able to Activate/Deactivate/Delete the device in their computer, they must activate it at the device where they want to set it up.
### Wireless Setup
* Step 1: Settings > Wi-Fi > Select “UCB Wireless” on the list.
* Step 2 : Open “ Browser” > Enter URL : “https://safeconnect.colorado.edu:9443/authenticate.!%5E”
(Reference : https://oit.colorado.edu/services/networking-internet-services/ucb-wireless)
* Step 3 : Please contact University Libraries - IT Department for account credentials to login to University Of Colorado - Boulder Wireless network. 
Time/Date Setup : Settings > Date & Time > Turn off “Automatic time zone” > “Select time zone” to “Denver GMT-07:00” > Turn on “Automatic time zone” 
Text-correction Setup : Settings > Language & Input > Android Keyboard (AOSP) > Text correction > Switch all off except “Block offensive words”

### Application Setup 
      Follow the steps below to set up the application in the tablet  :
* Step 1 : Open application name : “MLock”.
* Step 2 : Touch/Hold the top-right corner of the tablet for about 10 seconds then a password dialog will pop-up ( Do this step again if you don't see the password dialog - Be patient ). Then type the password (Keepass: Study Room Application/Hardware Tablet Pin).
* Step 3 : Press “Cookie” then turn on Cookie State ( Pink is ON, Gray is OFF ) and Press “< Cookie Setting” to go back.
* Step 4 : Press “Default App” and turn on “Auto Start” (Pink is ON, Gray is OFF) and Press “< Default App” to go back
* Step 5 : Press “Playback Setting” > “Web URL” and change the url         https://libapps.colorado.edu/room-booking-admin (PRODUCTION)
https://test-libapps.colorado.edu/room-booking-admin (TEST)
* Step 6 : Press the go back icon on the top-left ( )
* Step 7 : Login using the admin user credential in (Software setup) section above, then activate the device that you want to set up in the list by turn on the toggle button. () ( Pink is Activated, Gray is Deactivated).
* Step 8 : Do Step 2 and Step 5 but change the url to : https://libapps.colorado.edu/room-booking (PRODUCTION)
https://test-libapps.colorado.edu/room-booking (TEST)
* Step 9 : Press the go back icon on the top-left ( )
* Step 10 : Plug Magnetic Swipe Card device to the USB port.
* Step 11 : All set. 
* Step 12 (Optional) : Looking for an undergraduate student to try the system or if you are staff or graduate student you will be expecting an error message since the system is only allow undergraduate students to book the room. For Testing purpose: All staff can reserve the room.

## Hardware Questions 
* Tablet: techsupport@mimomonitors.com <techsupport@mimomonitors.com>
* Magnetic Swipe Card: Buff OneCard Office.

## Coding

* Room Booking application (Main Interface): https://github.com/culibraries/room-booking
* Room Booking admin (Admin Interface): https://github.com/culibraries/room-booking-admin
* API (Cybercomm): https://github.com/culibraries/room-booking-django-app

### Admin User !IMPORTANT 
In order to access to Room Booking admin application, you need to create a local user in Cybercomm with a password and add group `study-room-admin` to that user as the permission. Store the username and password in a safe place. You will need to use that to set up the tablet.

Basically, The tablet will interact with API (Cybercomm) using the Token from this admin user, which is stored in localStorage in each device. In Application Setup -> Step 5 (in the section above), The reason you need to go to room-booking-admin is to get the Token and store it in each tablet for future API call.

### Room Booking Admin
This application is to manage information in each tablet. You can add/delete/modify the information of each room which corresponding to each tablet in front of the room. However, you are not able to activate unless you access to this application in a tablet.
Dont forget to Generate Unique ID for each tablet.

### Room Booking
Local Development: 
* Step 1: Create a testing room in Libcal and get all of information including: location_id, space_id, hours_view_id.
* Step 2: Get Token string from admin user above, and libcal_token (It will be expired after 60 minutes.)
* Step 3: Store all of these variable in LocalStorage of the web browser.
* Step 4: Go to auth-guard.service.ts and remove line 26,27. (Don't forget to bring it back when deploy to TEST or PRODUCTION.

You can plugging the Magnetic Swipe Card into your computer via USB port and use it as a testing device. 

### API (Cybercom)
* Get libcal_token from Libcal using LIBCAL_CLIENT_ID and LIBCAL_CLIENT_SECRET (in Libops).
* Get information after swipe action using sierra api.

### Logs
All the log will be uploaded to S3 at cubl-log/room-booking as CSV format around midnight everyday.
Terms explaination in the log:
* referesh libcal_token: Since the token only good for 60 minutes, so it will need to recreated when it is expired. 
* app starting...: this happen when the application refresh the page. 
* card - PType - Room (email and time slots): when these 3 messages go together. It means someone successfully book the room.
* the library is closed: It means the tablet is not display for booking anymore since the library is closed.
Please looking the github code for more log information.

## Support Documentation For Circ Desk Staff

1. What should I do if the message “SYSTEM ERROR” appears?

      If a student reports to Circ Desk about the error above:
      * Step 1: Go to the tablet and press “Press here to reload”. If the problem isn’t fixed => Step 2.
      * Step 2: Reboot the tablet (Question 3).
      After trying Step 1 and 2, and the error keep showing up. Please manually reserve the room for that student via libcal, and submit a ticket to University Libraries - IT Department.

2. What should I do if there is an internet connection issue ?

      If internet connection still good and it keeps asking for username and password, please submit a ticket or contact to University Libraries - IT Department. 
      If there is no internet connectivity in the building, please contact OIT for internet connection issue.

3. How can i reboot the tablet ?

      Look for a switch button right underneath the tablet. That is the power button where you can turn on/off the device. You can simply turn it off and turn it back on to reboot the tablet. 

4. What should I do if the screen turns black ?

      * Touch the screen to see if its back on.
      * Check to see if there is any power outage.
      * Reboot the tablet (Question 3).
      Submit a ticket to University Libraries - IT Department if the screen is not back on.

5. Why student successfully reserve a room through Libcal but it is not showing on the tablet ?

      It will take around 45 seconds to 60 seconds for the tablet pull information from Libcal. Just wait for about that time. It should be updated on the tablet. As long as the student have the email confirmation from Libcal it should be fine.

6. If there is any issue that is not related to all of the topics above => try to reboot the table (Question 3) to see if it would be fixed. If not please manually reserve the room for the student via Libcal and submit a ticket to University Libraries - IT Department.

## TODO
* Network account (libkey) was disabled with password reset
* DDS Harware connect: share Mac address of device and connect through DDS. 

