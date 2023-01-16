# HA_emporia_vue_modify_yaml
Personal implementation of Home Assistant Emporia Vue Modified YAML Code using github://magico13/ha-emporia-vue as a starting point / baseline.

After flashing Emporia Vue device with ESPHome firmware I had to modify the YAML code for my needs.  If you have any suggestions, I would appreciate any feedback to improve the code or identify additional opportunities on how to use the data.

Emporia Vue supports the main circuit to my home with additional 16 CT clmaps which limits what I can actual monitor.  The energy monitor sensors in the Electrial Panel Breaker Layout identifies the Emporia Vue CT clamp associated with the circuits.  Generating a similar spread sheet will greatly help in understanding and debugging your particular implementation.  

Note that I have several circuits associated one Emporia Vue CT clamp.  For example, circuits 1, 5, 9 all go through the #3 CT clamp.  I tried to group circuits based on what they supply, frequency of use, how interested I’m in the actually data, and characteristics of the circuits. You can group them as long as they are from the same phase, you can physically clamp around the multiple wires, and less than anticipated AMP supported by the CT clamp. 

Also, I only clamp one phase on the dual phase breakers. Initially, I clamped both phases and ran the appliance through its paces to characterize the energy load. For example, my pool pump always measures ~2X the power used on one phase. So if I detect any load on the one phase, I just double it to account for the second phase.  My oven is more complex. The electronic display runs at ~6W all the time on phase A. The oven light runs about ~35-40 W on Phase A. The oven warming draw uses ~900W on only Phase B (which I don't monitor but can infer). And the oven when heating uses several different wattage levels but are ~2x on both phases with the addition of panel, light, and warming draw when used. Since the warming draw uses ~900W, I look at the remaining balance of energy after accounting for all monitored circuits.  It’s rare (never happens in my case) that I would run something on Phase B at ~900W that doesn’t also consume similar wattage on Phase A except for this warming draw.  After looking at the yaml code for the "oven_filter", this hopefully will become more clear.

I left the remaining circuits unmonitored that I didn't have a HA automation scenario I would want to enable, ciruits I couldn't easily pair up with another circuit due to physical wire location in the breaker panel, are rarely used, or generally low wattage mostly like lighting ciruits.  I actually calculate the “remaining” balance which is anything not monitored.  It is calculate as the difference between the main CT clamp wattage on each Phase minus all of the 16 CT clamp wattages.  Note that the "remaining" balance is less accurate since I have to make some estimates/guessimates for some of applicance ciruits which are not 100% accurate.  I have found that oversubscription is usually fairly small and sometimes can see negatives wattage.  But overall the results have been less than ~1%.

![Image 1-16-23 at 10 56 AM](https://user-images.githubusercontent.com/105085397/212719866-bfd74362-b9c0-419c-81c8-4dc385789fc3.jpg)

Data I currently generate from the Emporia Vue both measured and calculated: 

![325657210_1889030221436526_5505722108430067678_n](https://user-images.githubusercontent.com/105085397/212724349-131066e3-5dda-41b9-b201-39f228847678.jpg)

