# GRAFIKA KOMPUTER (A) - TUGAS 1

### Nama : Rumaisha Afrina | NRP : 5025221146


Pada tugas 1, saya membuat program segitiga sederhana menggunakan WebGL dengan modifikasi warna dan juga tulisan 2D. Program yang saya buat terdiri dari 2 file, yaitu `index.html` dan `app.js`. 

Berikut ini adalah kode untuk `index.html` :
```
  <html>
  <head>
      <title>Grafika Komputer - Tugas 1 WebGL</title>
      <style>
          #canvas-container {
              position: relative;
          }
          #canvas-game, #canvas-text {
              position: absolute;
              top: 0;
              left: 0;
          }
      </style>
  </head>
  <body>
      <div id="canvas-container">
          <canvas id="canvas-game" width="800" height="600"></canvas>
          <canvas id="canvas-text" width="800" height="600"></canvas>
      </div>
  
      <script src="app.js"></script>
  </body>
  </html>
```

Kode HTML ini membuat sebuah halaman web dengan judul `Grafika Komputer - Tugas 1 WebGL`. Di dalam halaman tersebut, terdapat sebuah div dengan id `canvas-container` yang berfungsi sebagai wadah untuk dua elemen canvas yang ditumpuk secara absolut. Kedua canvas tersebut memiliki id `canvas-game` dan `canvas-text`, masing-masing dengan lebar 800 piksel dan tinggi 600 piksel. Elemen canvas ini diatur dengan posisi absolut sehingga mereka menumpuk satu sama lain di dalam wadah. Selain itu, file JavaScript eksternal bernama `app.js` disertakan di akhir body untuk menambahkan fungsionalitas tambahan pada halaman ini.

Berikut ini adalah kode untuk `app.js` : 

```
var InitDemo = function(){
    console.log('Program ini berjalan dengan baik');

    var canvas = document.getElementById('canvas-game');
    var gl = canvas.getContext('webgl');

    if(!gl){
        console.log('WebGL tidak didukung, coba sesaat lagi');
        gl = canvas.getContext('experimental-webgl');
    }
    if(!gl){
        alert('Browser tidak mendukung WebGL');
        return;
    }

    var vertexShaderSource = `
    attribute vec2 vertPosition;
    attribute vec3 vertColor;
    varying vec3 fragColor;
    void main() {
        fragColor = vertColor;
        gl_Position = vec4(vertPosition, 0.0, 1.0);
    }`;

    var fragmentShaderSource = `
    precision mediump float;
    varying vec3 fragColor;
    void main() {
        gl_FragColor = vec4(fragColor, 1.0);
    }`;

    var vertexShader = gl.createShader(gl.VERTEX_SHADER);
    var fragmentShader = gl.createShader(gl.FRAGMENT_SHADER);

    gl.shaderSource(vertexShader, vertexShaderSource);
    gl.shaderSource(fragmentShader, fragmentShaderSource);

    gl.compileShader(vertexShader);
    if(!gl.getShaderParameter(vertexShader, gl.COMPILE_STATUS)){
        console.error('Error saat compile vertex shader!', gl.getShaderInfoLog(vertexShader));
        return;
    }

    gl.compileShader(fragmentShader);
    if(!gl.getShaderParameter(fragmentShader, gl.COMPILE_STATUS)){
        console.error('Error saat compile fragment shader!', gl.getShaderInfoLog(fragmentShader));
        return;
    }

    var program = gl.createProgram();
    gl.attachShader(program, vertexShader);
    gl.attachShader(program, fragmentShader);

    gl.linkProgram(program);
    if(!gl.getProgramParameter(program, gl.LINK_STATUS)){
        console.error('Error saat link program!', gl.getProgramInfoLog(program));
        return;
    }

    gl.validateProgram(program);
    if(!gl.getProgramParameter(program, gl.VALIDATE_STATUS)){
        console.error('Error saat validate program!', gl.getProgramInfoLog(program));
        return;
    }

    var segitigaVertices = [
        0.0, 0.5,    1.0, 0.7, 0.0,
        -0.5, -0.5,  0.2, 0.9, 1.0, 
        0.5, -0.5,   1.0, 0.2, 0.6
    ];

    var segitigaVertexBufferObject = gl.createBuffer();
    gl.bindBuffer(gl.ARRAY_BUFFER, segitigaVertexBufferObject);
    gl.bufferData(gl.ARRAY_BUFFER, new Float32Array(segitigaVertices), gl.STATIC_DRAW);

    var positionAttribLocation = gl.getAttribLocation(program, 'vertPosition');
    var colorAttribLocation = gl.getAttribLocation(program, 'vertColor');
    gl.vertexAttribPointer(
        positionAttribLocation,
        2,
        gl.FLOAT,
        gl.FALSE,
        5 * Float32Array.BYTES_PER_ELEMENT,
        0
    );
    gl.vertexAttribPointer(
        colorAttribLocation,
        3,
        gl.FLOAT,
        gl.FALSE,
        5 * Float32Array.BYTES_PER_ELEMENT,
        2 * Float32Array.BYTES_PER_ELEMENT
    );

    gl.enableVertexAttribArray(positionAttribLocation);
    gl.enableVertexAttribArray(colorAttribLocation);

    gl.useProgram(program);
    gl.clearColor(0.1, 0.10, 0.1, 1.0);
    gl.clear(gl.COLOR_BUFFER_BIT | gl.DEPTH_BUFFER_BIT);
    gl.drawArrays(gl.TRIANGLES, 0, 3);

    // Get the 2D context of the second canvas
    var textCanvas = document.getElementById('canvas-text');
    var ctx = textCanvas.getContext('2d');

    // Draw the text
    ctx.font = '30px Inter';
    ctx.fillStyle = 'white';
    var text = 'This is a simple triangle program';
    var textWidth = ctx.measureText(text).width;
    var canvasWidth = textCanvas.width;
    var xPosition = (canvasWidth - textWidth) / 2;
    var upperSpacing = 50;
    ctx.fillText(text, xPosition, 30 + upperSpacing);
};

window.onload = function() {
    InitDemo();
};
```

Kode JavaScript ini menginisialisasi sebuah demo WebGL yang menggambar sebuah segitiga berwarna pada elemen canvas dengan id `canvas-game`. Pertama, kode memeriksa dukungan WebGL pada browser dan mengatur shader vertex dan fragment untuk menggambar segitiga. Setelah itu, kode membuat buffer untuk menyimpan data vertex segitiga dan menghubungkannya dengan atribut shader. Program WebGL kemudian digunakan untuk menggambar segitiga pada canvas. Selain itu, kode juga menggambar teks pada elemen canvas kedua dengan id `canvas-text` menggunakan konteks 2D. Teks tersebut ditampilkan di tengah canvas dengan font dan warna yang telah ditentukan.

## Hasil
Berikut ini adalah hasil dari program sederhana yang telah saya buat :
