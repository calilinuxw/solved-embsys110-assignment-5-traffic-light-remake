Download Link: https://assignmentchef.com/product/solved-embsys110-assignment-5-traffic-light-remake
<br>



In classes students learned about how a system can be partitioned into components, and how different components communicate with each other via well-defined event interfaces. In this exercise students put what they learned into practice by hooking up multiple components to form a functional system.

<h1>2         Readings</h1>

<ol>

 <li>Finite State Machines for Integration and Control in ALICE. Giacinto De Cataldo, etc.</li>

</ol>

Proceedings of ICALEPCS07, Knoxville, Tennessee, USA. 2007.

<a href="https://accelconf.web.cern.ch/accelconf/ica07/PAPERS/RPPB21.PDF">(</a><a href="https://accelconf.web.cern.ch/accelconf/ica07/PAPERS/RPPB21.PDF">https://accelconf.web.cern.ch/accelconf/ica07/PAPERS/RPPB21.PDF</a><a href="https://accelconf.web.cern.ch/accelconf/ica07/PAPERS/RPPB21.PDF">)</a>

This paper illustrates the use of a hierarchy of finite state machines in ALICE (A Large Ion Collider Experiment) on the Large Hadron Collider (LHC) in CERN. This is one of the many examples of the application of formal state machines in critical systems. Note that in this system each state machine itself is not hierarchical.

<ol start="2">

 <li>Use of statecharts in the modelling of dynamic behaviour in the ATLAS DAQ prototype-1. P. Croll’, P.-Y. Duval’, R. Jones3, S. K010s4, R.F. Sari’ and S. Wheeler. IEEE Transactions on Nuclear Science, Vol. 45, No. 4, August 1998.</li>

</ol>

<a href="https://www.researchgate.net/publication/3136235_Use_of_statecharts_in_the_modelling_of_dynamic_behaviour_in_the_ATLAS_DAQ_prototype-1">(</a><a href="https://www.researchgate.net/publication/3136235_Use_of_statecharts_in_the_modelling_of_dynamic_behaviour_in_the_ATLAS_DAQ_prototype-1">https://www.researchgate.net/publication/3136235_Use_of_statecharts_in_the_modelling_of_d </a><a href="https://www.researchgate.net/publication/3136235_Use_of_statecharts_in_the_modelling_of_dynamic_behaviour_in_the_ATLAS_DAQ_prototype-1">ynamic_behaviour_in_the_ATLAS_DAQ_prototype-1</a><a href="https://www.researchgate.net/publication/3136235_Use_of_statecharts_in_the_modelling_of_dynamic_behaviour_in_the_ATLAS_DAQ_prototype-1">)</a>

This earlier paper documents the evaluation of employing statecharts and CHSM tools in ALICE on LHC. Searching for CHSM on Github still yields active projects.

<h1>3         Setup</h1>

<ol>

 <li><em>Carefully</em> plug in the following extension modules in the exact order onto your Nucleo board:

  <ul>

   <li>X-NUCLEO-IKS01A1 (IMU sensor).</li>

   <li>X-NUCLEO-IDW04A1 (WiFi).</li>

   <li>LCD module.</li>

  </ul></li>

</ol>

Please ensure you align the pins correctly and do not bend any pins.

In this assignment we are only using the LCD module. Plugging in other modules now avoids the need to unplug the LCD module later and gives us more space to press the USER (blue) button.

<ol start="2">

 <li>Download the compressed project file (platform-stm32f401-nucleo_assignment5.tgz) from the Assignment 4 course site.</li>

</ol>

Place the downloaded tgz file in your VM under <strong>~/Projects/stm32</strong>.

<ol start="3">

 <li>Move your existing project folder to a backup folder, e.g.</li>

</ol>

mv platform-stm32f401-nucleo platform-stm32f401-nucleo.bak

Note – You may want to pick another backup folder name if the one shown above already exists.

<ol start="4">

 <li>Decompress the tgz file with:</li>

</ol>

tar xvzf platform-stm32f401-nucleo_assignment5.tgz

The project folder will be expanded to <strong>~/Projects/stm32/platform-stm32f401-nucleo</strong>.

<ol start="5">

 <li>Launch Eclipse. Hit F5 to refresh the project content.</li>

</ol>

Or you can right-click on the project in Project Explorer and then click “Refresh”.

<ol start="6">

 <li>Clean and rebuild the project. Download it to the board and make sure it runs.</li>

</ol>

In minicom, you should see log statements printed out by the USER_BTN component when you press the USER button. They look like these:

<ul>

 <li>USER_BTN(23): Inactive TRIGGER from UNDEF(0) seq=30</li>

 <li>USER_BTN(23): Inactive EXIT</li>

 <li>USER_BTN(23): Active ENTRY</li>

 <li>USER_BTN(23): PulseWait ENTRY</li>

</ul>

The LCD shows a white background which is normal.

<ol start="7">

 <li>If you have not installed UMLet, you will need to do so for this assignment. Please refer to the Setup notes in the previous assignment.</li>

</ol>

<h1>4         Tasks</h1>

Our demo system involves the following components:

<ul>

 <li>System (SYSTEM) – System manager coordinating other components.</li>

 <li>GpioIn (USER_BTN) – GPIO input connected to the USER button.</li>

 <li>Traffic (TRAFFIC) – Traffic light controller with two Lamp (LAMP_NS and LAMP_EW) orthogonal regions.</li>

 <li>Disp/Ili9431 (ILI9341) – Display controller.</li>

</ul>

The following diagram shows the control hierarchy of these components.

Tasks for this assignment are:

<ol>

 <li>In <strong>src/System/System.cpp</strong> <em>Started</em> state, handle GPIO_IN_PULSE_IND and GPIO_IN_HOLD_IND by sending the TRAFFIC_CAR_NS_REQ and TRAFFIC_CAR_EW_REQ respectively.</li>

</ol>

This simulates a car arriving along the NS direction with a short press (&lt;1s) on the USER button. It simulates a car arriving along the EW direction with a hold (&gt;1s) on the USER button. If you keep holding the button, it simulates cars keep arriving along the EW direction.

In this example, the System acts as a coordinator between the GpioIn component and the Traffic component

Hint: You can send an event like the following example:

Evt *evt = new TrafficCarEWReq(TRAFFIC, GET_HSMN());

Fw::Post(evt);

<ol start="2">

 <li>In <strong>src/Traffic/Lamp/Lamp.cpp</strong>, call the Draw() member function at appropriate places to draw traffic lights on the display module. See the following function for details of the implementation:</li>

</ol>

void Lamp::Draw(Hsmn hsmn, bool redOn, bool yellowOn, bool greenOn)

<ol start="3">

 <li>Test your code by pressing and holding the USER button. Ensure the traffic lights on the LCD displays show the expected patterns. Use log messages on minicom to debug and verify your application.</li>

</ol>

Note – You can control log output with the “<strong>log on/off</strong>” command. You can check component states and component numbers (HSMN) with the “<strong>state</strong>” and “<strong>hsm</strong>” command.