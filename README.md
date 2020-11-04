# domoticz-weekly-planning
Weekly programming functionality allow to view a graphic table in the Domoticz timers page

It can be used for **light/switch(ON/OFF)**, **selector levels** and **SetPoints** 
- a program can be defined on 2 weeks with the **odd & even weeks mode**
- the time range can be changed from 1 hour to 2 mins
- a entire program can be deactivate without deleting all timers and reactivate later with the buttons **Deactivate All** and **Activate All**


#### Setpoint sample:
![setpoint](/images/setpoint_tabs.png)
In setpoint configuration, you can define your own temperatures values for the 4th buttons. You just have to click inside the button and enter your value from to greater to the lower

#### Selector switch sample: Electric radiator
![Electric heater](/images/electric_heater_timers.png)
![Timers](/images/selectorswitch_electric_heater.png)

#### Switch samples:
![Switch sample](/images/switch_15mins.png)
![Switch Odd/Even](/images/switch_odd_even_week.png)

////////////////////////////////////////////////////////////

## Automatic installation:
To download files on domoticz  folder, first move with a command line to the domoticz folder: 

wget https://raw.githubusercontent.com/syrhus/domoticz-weekly-planning/master/www/app/timers/DeviceTimersController.js  -O www/app/timers/DeviceTimersController.js

wget https://raw.githubusercontent.com/syrhus/domoticz-weekly-planning/master/www/app/timers/planning.js  -O www/app/timers/planning.js

wget https://raw.githubusercontent.com/syrhus/domoticz-weekly-planning/master/www/css/planning.css -O www/css/planning.css

wget https://raw.githubusercontent.com/syrhus/domoticz-weekly-planning/master/www/views/timers.html -O www/views/timers.html


NOTE: optional, I created a "empty" html5.appcache file as it is absolutely useless = no Offline mode.
you can forget it 
wget https://raw.githubusercontent.com/syrhus/domoticz-weekly-planning/master/www/html5.appcache -O www/html5.appcache


/////////////////////////////////////////////////////////////

## Manual installation:
Add files:
   - **planning.js** file to the **domoticz/www/app/timers** folder 
   - **planning.css** file to the **domoticz/www/css** folder
   - **timers.html** file to the **domoticz/www/views** folder

Edit the following files:

domoticz/www/app/timers/**DeviceTimersController.js**

add the path to planning javascript file " **,'timers/planning'** "
```javascript
define(['app', 'timers/factories', 'timers/components','timers/planning' ], function (app) {
```

1- add the trigger event **timersInitialized** before the call **refreshTimers();**

2- add the trigger event **timersLoaded** in the **refreshTimers** function

```javascript
.....
     init();

    
     function init() {
         .........
         deviceApi.getDeviceInfo(vm.deviceIdx).then(function (device) {
             .........

             $( document ).trigger( "timersInitialized", [vm, refreshTimers] );//<===Update for Planning
             refreshTimers();
         });

         vm.typeOptions = deviceTimerOptions.timerTypes;
         vm.timerSettings = deviceTimerConfigUtils.getTimerDefaultConfig();
     }
         
     function refreshTimers() {
         vm.selectedTimerIdx = null;

         deviceTimers.getTimers(vm.deviceIdx).then(function (items) {
             vm.timers = items;
             $( document ).trigger( "timersLoaded", [items] );;//<===Update for Planning
         });
     }
```
