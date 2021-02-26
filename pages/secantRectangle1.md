# :::: clue
# --outlinebox
##### Car Ride
The car is at position 5 meters at time $t=0$ and follows the velocity curve given.  Estimate how far the car travels between time $t=0$ and time $t=8$.  What is the car's position at time $t=8$?  Round your answers to the nearest whole number.
# --outlinebox
# ::::

# :::: notes
# --partialborder
##### Note
We know that the area of each rectangle estimates the distance traveled during the rectangle's time period.  If we add up all the rectangle areas, we can estimate the total distance traveled.  What if you didn't have any way to add up the areas of the rectangles?  How else can we get the distance traveled? 
# --partialborder
# ::::

[Problem](::clue/button,transparent,draggable,closeable,center,lightbox,outline,shadow) [note](::notes/button,transparent,draggable,outline,closeable,center,shadow) [Submit Solution](:=compute=true) Here. You have a weird secant rectangle array. 
# :::: answerbar
distance traveled between $t=0$ and $t=8$ [](:?s1)m  position at $t=8$ [](:?s2)m 
# ::::
# :::: toolbar
```javascript /autoplay/p5js
///////////////////////////////////////////////////////////////////
// this is a script for a RESOURCE PANEL toolbar
///////////////////////////////////////////////////////////////////


// import the calc library
//smartdown.import=/assets/libs/toolbar.js

const myDiv = this.div;
myDiv.style.background = '#FFFFFF';
myDiv.style.borderRadius = '8px';

let maxButtons = 6;
let xSpacer = 10, ySpacer = 5, width = 40;
let B = new ResourcePanel(p5);

///////////////////////////////////////////////////////////////////
// Add the buttons you want on your resource panel
// NOTE:  The value of page variable env.numButtons should match the number of buttons you add here

B.addButton('secant rectangle array'); 

smartdown.setVariable('numButtons', 1);  // keep track of number of buttons

///////////////////////////////////////////////////////////////////


p5.setup = function() { 
  let canvasWidth = xSpacer + (width + xSpacer) * maxButtons;  // set the size of the playable
  let canvasHeight = width + 3 * ySpacer;
  p5.createCanvas(canvasWidth,canvasHeight);     // create the canvas we will draw on
};


p5.draw = function() {  // draw the buttons
  B.draw(p5.mouseX,p5.mouseY);
}


p5.mousePressed = function()     // this function is called everytime the user clicks the mouse
{
  const [bType, state, id] = B.mousePressed();  // returns info on the button pushed
  
  if (id != -1) {                              // if it's a valid button
    smartdown.setVariable('buttonType', bType);         // set page mode correct type of button
    smartdown.setVariable('active', true);     // alert page we have an active button
  }
  
  // p5.loop();    // EnergyHack to enable looping for duration of drag.
};


p5.mouseReleased = function() {     // this function is called when the user releases the mouse
                                    // button after a click.

  // EnergyHack to stop looping 5 sec after release.
//  window.setTimeout(function() {
//    p5.noLoop();
//  }, 5000);
};


//////////////////////////////////////////////////////////////////////////////
// Event handling

smartdown.setVariable('buttonType', 0);
smartdown.setVariable('active', false);

this.dependOn = ['numButtons'];  // if we're removing buttons the numButtons will go down
this.depend = function() {
  
  if (env.numButtons < B.size()) {  // If we have more buttons than the page has elements, delete the active button
     B.removeActiveButton();  // we've used this resource
     smartdown.setVariable('active', false);
     if (B.size() == 0) { p5.noLoop(); }
  }

  if (B.size() == 0) {
    smartdown.hideDisclosure('toolbar','','');
    smartdown.showDisclosure('answerbar','','transparent');
  }
};

```
# ::::

# :::: success
# --outlinebox
Success! 
[Continue](/pages/secantRectangle2)
# --outlinebox
# ::::


```javascript /autoplay
//smartdown.import=https://cdnjs.cloudflare.com/ajax/libs/jsxgraph/0.99.7/jsxgraphcore.js

smartdown.importCssUrl('https://cdnjs.cloudflare.com/ajax/libs/jsxgraph/0.99.7/jsxgraph.css');

// import the calc library
//smartdown.import=/assets/libs/calc.js

smartdown.showDisclosure('toolbar','','transparent');

const myDiv = this.div;
myDiv.style.width = '100%';
myDiv.style.height = '100%';
myDiv.style.margin = 'auto';
myDiv.innerHTML = `<div id='top' style='height:100px; width:100%; border:1px solid gray;background:#EEFFCC;border-radius:8px;'></div><div id='bottom' style='height:600px; width:100%; border: 1px solid gray;background:#FFFFFF;border-radius:8px;';></div>`;

let xlow = -2;
let xhigh = 12;
let ylow = -5;
let yhigh = 60;

let workspace = new Workspace('bottom', [xlow,yhigh,xhigh,ylow], 
  { xlabel:'time (s)', ylabel:'velocity (m/s)'});

let F = new ProblemFunction(function(x) {  return -Math.pow(x,3)/8 + Math.pow(x,2); }, 
  'velocity of car', 3.5, [0,xhigh], [0,8]);
let F_id = workspace.addFunction(F);






////////////////////////////////////////////////////////////////////////////////////




/////////////////////////////////////////////////////////////////////////////////////////
// second board


let board1 = JXG.JSXGraph.initBoard('top', {boundingbox:[-20,5,120,-2], keepaspectratio:false, axis:false, showCopyright:false, showNavigation:false});

workspace.board.addChild(board1);

let xaxis1 = board1.create('axis', [[0, 0], [1,0]], 
  {name:'meters', 
    withLabel: true,
    label: {
      fontSize: 20,
      position: 'rt',  // possible values are 'lft', 'rt', 'top', 'bot'
      offset: [-80, 20]   // (in pixels)
    },
    ticks: {visible:false}
  });

let yaxis1 = board1.create('axis', [[0, 0], [0, 1]], 
  {name:'', 
    withLabel: false, 
    label: {
      fontSize: 20,
      position: 'rt',  // possible values are 'lft', 'rt', 'top', 'bot'
      offset: [-120, -20]   // (in pixels)
    },
    ticks: {visible:false}
  }); 


let carurl = 'https://gist.githubusercontent.com/wildthinkslaboratory/ac98c0bb68ccf7528dc39fa1922d2bdb/raw/9e01e8197b3bf685747ae134de3d75feb64ea6f4/car.png';
let car = board1.create('image',[carurl, [function() { return workspace.area() + 1 ; },-0.2], [4,2]]);

let p2 = board1.create('point',[0, 2.2],{visible:false});
let p3 = board1.create('point',[function() { return workspace.area(); }, 2.2],{visible:false});

let dimensionLine = board1.create('segment', [p2,p3], {
  strokeColor:'#999999', 
  strokeWidth:2, 
  firstArrow:true, 
  lastArrow:true, 
  visible:true});


/////////////////////////////////////////////////////////////////////////////////////////


let useButton = function(mouseX, buttonType) {
  let pixelwidth = window.innerWidth * widthPercent;
  let margin = (window.innerWidth - pixelwidth)/2;
  let percent = (mouseX - margin) / pixelwidth;


  let boardwidth = xhigh - xlow;
  let x1 = xlow + boardwidth * percent;
  let x2 = x1 + boardwidth * 0.4;
  if (x2 > xhigh) { x2 = xhigh; }
  let xint = new XInterval(workspace.board, x1, x2);
  let srarray = new SecRectAdjArray(workspace.board, xint, F.f, 4, {
    change:'distance',
    units:'time',
    rate:'rate',
    annotations:'secant',
    attachButtonVisible:false,       
  });

  workspace.addElement(srarray);
 
  smartdown.setVariable('numButtons', env.numButtons - 1);  // keep track of resources
};


this.div.onmousedown = function(e) { 
  if (env.numButtons > 0 && env.active) {
    useButton(e.clientX, env.buttonType);
  }
};


let widthPercent = 0.8;
let heightPercent = 0.7;
let heightRatio = 1/6;

this.sizeChanged = function() {
  workspace.board.resizeContainer(window.innerWidth * widthPercent, (1-heightRatio) * window.innerHeight * heightPercent);       
  board1.resizeContainer(window.innerWidth * widthPercent, heightRatio * window.innerHeight * heightPercent);
};

this.sizeChanged();


workspace.board.on('update', function() {
  workspace.onUpdate();              // hook up workspace update functions
});

smartdown.setVariable('error', 100);
smartdown.setVariable('undo', false);

this.dependOn = ['undo', 'active'];
this.depend = function() {

  if (env.active == true) {
    myDiv.style.cursor = "url('/assets/images/calculus/SRCursor.svg'), auto";
  }
  else {
    myDiv.style.cursor = "default";
  }
};



```

# :::: keeptrying
# --colorbox
[](:!hint) 
# --colorbox
# ::::

```javascript /autoplay
//smartdown.import=/assets/libs/mapping.js

function removeEnterFromSmartdownString(name, smartdownVar) {
  if (smartdownVar[smartdownVar.length - 1] === '\n') {           
    smartdown.setVariable(name, smartdownVar.replace(/\s/g, ''));
  }
}

smartdown.setVariable('s1','');
smartdown.setVariable('s2','');
smartdown.setVariable('hint', 'Keep trying');
smartdown.setVariable('compute', false);

let answer1 = new ProblemAnswer(['44'], []);

let answer2 = new ProblemAnswer(['49'], 
  [['equals','44','Close, but think about the initial conditions of the problem.']]
  );


this.dependOn = ['compute', 's1', 's2'];  
this.depend = function() {

  removeEnterFromSmartdownString('s1', env.s1);
  removeEnterFromSmartdownString('s2', env.s2);


  if (env.compute == true) {
    smartdown.setVariable('compute', false);
    if (answer1.checkAnswer(env.s1) && answer2.checkAnswer(env.s2)) {
      smartdown.showDisclosure('success','','center,transparent,draggable,closeable,outline,shadow');
    }
    else {
    smartdown.setVariable('hint', answer2.checkHints(env.s2));
      smartdown.showDisclosure('keeptrying','','transparent,bottomright,shadow');
      setTimeout(function () {
        smartdown.hideDisclosure('keeptrying','','');
      }, 5000);
    }
  }
};
```

