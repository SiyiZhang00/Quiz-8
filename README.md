# Quiz-8
This is the assignment content of quiz8.
# Quiz-8
This is the assignment content of quiz8.

Part 1
Technique: Visible mending (Boro / Sashiko)

My inspiration comes from visible mending (Boro/Sashiko). The background is a regular woven grid that shows signs of wear and tear over time. The damaged areas are depicted as fabric tears and then sewn up with a single line, leaving behind gnarly stitches at the seams. As more damage accumulates, more seams are created. With the accumulation of damage, the restrained indigo color palette and the grayish-white stitches make the seams clear and legible. I will present a narrative from hairline wear to bold patches, demonstrating how repeated mending preserves and alters the whole.

Part 2
Technique: Procedural holes + contour extraction
The effect is achieved through a time-varying scalar field and marching cubes. OpenSimplex noise3D(xoff, yoff, zoff) fills the network. For each cell, getState() maps the four corners to a case, and switches to draw the correct contour segments, showing the constantly changing "tearing" outline. Place this contour layer on the woven mesh, and then use p5's drawingContext.setLineDash to redraw the edges for grayish-white stitching lines.

![Boro/Sashiko 1](assets/boro1.jpg)
![Boro/Sashiko 2](assets/boro2.jpg)


![Marching Squares demo](assets/ms-demo.jpg)

## References / Example code
- <link to museum page>
- <link to Coding Train Marching Squares>
- <link to p5 erase() / drawingContext / MDN setLineDash()>


code link: https://editor.p5js.org/codingtrain/sketches/MDJ61g-Xg?
code:
let res = 10;
let grid;
let cols, rows;
let onoise;
let xoff, yoff;
let zoff = 0;
let increment = 0.1;

function make2DArray(cols, rows) {
  let arr = new Array(cols);
  for (let i = 0; i < arr.length; i++) {
    arr[i] = new Array(rows);
  }
  return arr;
}


function setup() {
  createCanvas(600, 400);
  onoise = new OpenSimplexNoise(100);
  cols = floor(width / res) + 1;
  rows = floor(height / res) + 1;
  grid = make2DArray(cols, rows);
}

function draw() {
  let xoff = 0;
  for (let i = 0; i < cols; i++) {
    let yoff = 0;
    for (let j = 0; j < rows; j++) {
      let n = onoise.noise3D(xoff, yoff, zoff);
      grid[i][j] = n;
      yoff += increment;
    }
    xoff += increment;
  }
  zoff += increment * 0.05;



  background(127);
  for (let i = 0; i < cols; i++) {
    for (let j = 0; j < rows; j++) {
      stroke(map(grid[i][j], -1, 1, 0, 255));
      strokeWeight(4);
      point(i * res, j * res);
    }
  }

  for (let i = 0; i < cols - 1; i++) {
    for (let j = 0; j < rows - 1; j++) {
      let state = getState(ceil(grid[i][j]),
        ceil(grid[i + 1][j]),
        ceil(grid[i + 1][j + 1]),
        ceil(grid[i][j + 1]));
      let x = i * res;
      let y = j * res;
      stroke(255);
      strokeWeight(1);
      switch (state) {
        case 0:
          break;
        case 1:
          line(x, y + res / 2, x + res / 2, y + res);
          break;
        case 2:
          line(x + res, y + res / 2, x + res / 2, y + res);
          break;
        case 3:
          line(x, y + res / 2, x + res, y + res / 2);
          break;
        case 4:
          line(x + res / 2, y, x + res, y + res / 2);
          break;
        case 5:
          line(x, y + res / 2, x + res / 2, y);
          line(x + res / 2, y + res, x + res, y + res / 2);
          break;
        case 6:
          line(x + res / 2, y, x + res / 2, y + res);
          break;
        case 7:
          line(x + res / 2, y, x, y + res / 2);
          break;
        case 8:
          line(x, y + res / 2, x + res / 2, y);
          break;
        case 9:
          line(x + res / 2, y, x + res / 2, y + res);
          break;
        case 10:
          line(x + res / 2, y, x + res, y + res / 2);
          line(x, y + res / 2, x + res / 2, y + res);
          break;
        case 11:
          line(x + res / 2, y, x + res, y + res / 2);
          break;
        case 12:
          line(x, y + res / 2, x + res, y + res / 2);
          break;
        case 13:
          line(x + res, y + res / 2, x + res / 2, y + res);
          break;
        case 14:
          line(x, y + res / 2, x + res / 2, y + res);
          break;
      }
    }
  }
}



function getState(a, b, c, d) {
  return a * 8 + b * 4 + c * 2 + d * 1;
}
