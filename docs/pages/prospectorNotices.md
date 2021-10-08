# Prospector Print Templates troubleshooting

Notices from Prospector were printing incorrectly when processed through Sierra on some machines and for some users. Notices were printed on only half the page. Difficulty in that this problem can only be recreated when there are items to process

Problem-solving steps

1. Typically this problem is caused by a corrupt or out of date printer driver for the receipt printers
1. Updated Epson receipt printer driver
1. Solved the problem for the next batch of notices
1. I looked at `yynorpro` Sierra account was set to revert to previous settings at log-in, set these to save current settings and tested
1. Looked at preferred templates for `yynprpro` logins and found that these were **not** the same across machines where the `yynorpro` account was logged in. Changed/confirmed all accounts were set to use the Prospector template and saved settings
1. Tested and Prospector accounts were all printing correctly

I think that the problem was initially the USB drivers, but in the process of self-troubleshooting, that other settings were changed to get the slips to print correctly. 
