### Turning the Secant Into a Tangent

#### --outlinebox outer1

#### --outlinebox left1


#### --outlinebox


#### --outlinebox right1

Let's watch again as the secant line gets very small.  [secant to tangent](:=toTangent=true) [Reset](:=reset=true)
As the value of $h$ gets arbitrarily small but never reaches $0$, our secant turns into a **tangent** line.  

The derivative at $t=1$ is [](:?s1)
The derivative at $t=3$ is [](:?s2)
The derivative at $t=5$ is [](:?s3)

[Continue](/pages/derivative4)



#### --outlinebox
#### --outlinebox

 

```javascript /autoplay

const outer = document.getElementById('outer1');
const left = document.getElementById('left1');
const right = document.getElementById('right1');

outer.classList.remove('decoration-outlinebox');
left.classList.remove('decoration-outlinebox');
right.classList.remove('decoration-outlinebox');

outer.classList.add('outer-multi-col');
left.classList.add('playable-2-col');
right.classList.add('text-2-col');


//smartdown.import=https://cdnjs.cloudflare.com/ajax/libs/jsxgraph/0.99.7/jsxgraphcore.js
smartdown.importCssUrl('https://cdnjs.cloudflare.com/ajax/libs/jsxgraph/0.99.7/jsxgraph.css');
// import the calc library
//smartdown.import=/assets/libs/calc.js
//smartdown.import=/assets/libs/mapping.js


left.innerHTML = `<div id='top' style='height:100px; width:100%; border:1px solid gray;background:#EEFFCC;border-radius:8px;'></div><div id='bottom' style='height:600px; width:100%; border: 1px solid gray;background:#FFFFFF;border-radius:8px;';></div>`;

let xlow = -1.5;
let xhigh = 9;
let ylow = -10;
let yhigh = 81;

let workspace = new Workspace('bottom', [xlow,yhigh,xhigh,ylow],{ xlabel:'', ylabel:'' });
let F = new ProblemFunction(function(x) { return x * x; }, '', 4, [0,xhigh], []);
let F_id = workspace.addFunction(F);

let xint = new XInterval(workspace.board, 5,7);
let secant = new Secant(xint, F.f, {showUnits:true, 
  annotations:'on',  
  noChangeNumber:true,
  noUnitsNumber:true,
  change:'(5+h)^2 - 5^2',
  units:'h'
});
workspace.addElement(secant);

secant.xint.x1.setAttribute({name:'5'});
secant.xint.x2.setAttribute({name:'5 + h'});

// a glider that moves the car
let t = workspace.board.create('glider', [0,0, workspace.xaxis], {name:'', face:'^', size:12, color:'green'});
let p = workspace.board.create('point', [
  function() { return t.X(); }, 
  function() { return F.f(t.X()); }], {color:'green', name:''});

// get the color scheme for our extra workspace objects
let colors = new BlueTheme();

// tangent stuff 
let d = function(x) { return 2*x; }


let t2 = workspace.board.create('glider', [1,0, workspace.xaxis], 
  {name:'', face:'^', size:12, color:colors.fill, visible:false});
let p2 = workspace.board.create('point', [
  function() { return t2.X(); }, 
  function() { return d(t2.X()); }], {color:colors.fill, name:'', visible:false});
let p3 = workspace.board.create('point', [
  function() { return t2.X(); }, 
  function() { return F.f(t2.X()); }], {color:colors.fill, name:'', visible:false});

let tangent = workspace.board.create('line', [
  function() { return F.f(t2.X());},
  function() { return - d(t2.X());},1], {color:colors.stroke, visible:false});


// some fabulous hackery to figure out the placement of the text
let fakeText = workspace.board.create('text', [0,ylow - 2,'slope = 2.02'],{visible:true});
let tWidth = textWidth(fakeText, workspace.board);
let Xerror = (xhigh - xlow)/50;

let tangentSlopeText = workspace.board.create('text',[
  function() { return t2.X() - tWidth - 3 * Xerror; },
  function() { return F.f(t2.X());},
  function(){ return 'slope = '+ d(t2.X()).toFixed(2); }], {
    color:colors.lightAnnote,
    fontSize:colors.fontSizeAnnote, 
    visible:false
  });



// animation secant into tangent
let animationTime = 2000;
let animationCallBack = function() {
  secant.hide();
  t2.setAttribute({visible:true});
  p3.setAttribute({visible:true});
  tangent.setAttribute({visible:true});
  tangentSlopeText.setAttribute({visible:true});

};

let goClose = function() {
  t2.moveTo([secant.xint.x1.X(),0]);
  if (secant.xint.X2() < secant.xint.X1()) {
    secant.xint.x2.moveTo(
      [secant.xint.X1()-0.01, 0],
      animationTime, 
      {effect: '--', callback: animationCallBack } );
  }
  else {
    secant.xint.x2.moveTo(
      [secant.xint.X1()+0.01, 0],
      animationTime, 
      {effect: '--', callback: animationCallBack } );
  }
}


let resetSecant = function() {
  secant.xint.x1.moveTo([5,0]);
  secant.xint.x2.moveTo([7,0]);
};

/////////////////////////////////////////////////////////////////////////////////////////
// second board


let board1 = JXG.JSXGraph.initBoard('top', {boundingbox:[-13.5,5,81,-2], keepaspectratio:false, axis:false, showCopyright:false, showNavigation:false});

workspace.board.addChild(board1);

let xaxis1 = board1.create('axis', [[0, 0], [1,0]], 
  {name:'meters', 
    withLabel: true,
    label: {
      fontSize: 20,
      position: 'rt',  // possible values are 'lft', 'rt', 'top', 'bot'
      offset: [-80, 20]   // (in pixels)
    }
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
let car = board1.create('image',[carurl, [function() { return F.f(t.X()) -4 ; },-0.2], [4,2]]);


////////////////////////////////////////////////////////////////////////////////////

workspace.board.on('update', function() {
  workspace.onUpdate();
});


let heightPercent = 0.7;
let heightRatio = 1/6;

this.sizeChanged = function() {
  workspace.board.resizeContainer(left.offsetWidth, (1-heightRatio) * window.innerHeight * heightPercent);
  board1.resizeContainer(left.offsetWidth, heightRatio * window.innerHeight * heightPercent);
};


this.sizeChanged();


smartdown.setVariable('toTangent', false);
smartdown.setVariable('reset', false);

this.dependOn = ['toTangent', 'reset'];
this.depend = function() {

  if (env.toTangent == true) {
    smartdown.setVariable('toTangent', false);
    goClose();
  }
  if (env.reset == true) {
    smartdown.setVariable('reset', false);
    resetSecant();
    secant.show();
    t2.setAttribute({visible:false});
    p3.setAttribute({visible:false});
    tangent.setAttribute({visible:false});
    tangentSlopeText.setAttribute({visible:false});
  }
};

//////////////////////////////////////////////////////////////// NOTATION MAPPING

outer.classList.add('outer-multi-col');
left.classList.add('playable-2-col');
right.classList.add('text-2-col');

// set up highlight mapping for formulas.  connect them with their
// model highlight
const formula1 = document.getElementById('MathJax-Element-7-Frame');
formula1.onmouseover = onWideAFFactory(formula1, showAFFactory([dfhighlight]));
formula1.onmouseout = offWideAFFactory(formula1, hideAFFactory([dfhighlight]));
formula1.classList.add('highlightOffNarrow');



```