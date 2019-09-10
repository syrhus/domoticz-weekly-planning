# domoticz-weekly-planning
Weekly programming functionality allow to view a graphic table in the Domoticz timers page

It can be used for light/switch(ON/OFF), selector levels and SetPoints
- a program can be defined on 2 weeks with the odd & even weeks mode
- the time range can be changed from 1 hour to 10 mins
- a entire program can be deactivate without deleting all timers and reactivate later with the buttons **Deactivate All** and **Activate All**

/////////////////////////////////////////////////////////////

Manual installation:
Add files:
   - **planning.js** file to the **domoticz/www/app/timers** folder 
   - **planning.css** file to the **domoticz/www/css** folder

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
