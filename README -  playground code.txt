
// 
// Cube3d
// 

var createScene = function () {
    var scene = new BABYLON.Scene(engine);  // new scene
    scene.clearColor = new BABYLON.Color3(0.8, 0.8, 0.8);  // set new default color

    // ArcRotateCamera("name", alpha_angle, beta_angle, radius, target_position);
    var camera = new BABYLON.ArcRotateCamera(
        "Camera",
         -Math.PI / 2,
          Math.PI / 2,
          12,
          new BABYLON.Vector3(0, 5, 28)  // position the camera
    );
    camera.setTarget(BABYLON.Vector3.Zero());  // This aims the camera to scene origin
    // set camera restrictions using alpha/beta limits (using radians instead of Math.PI)
    camera.lowerBetaLimit = BABYLON.Tools.ToRadians(30);  // camera not too high (Beta 0 straight up)
    camera.upperBetaLimit = BABYLON.Tools.ToRadians(85);  // camera limit just above ground level (Beta 90 horizon)
    camera.attachControl(canvas, true);  // This attaches the camera to the canvas

    // MeshBuilder built-in ground (name, dimensions)
    var ground = BABYLON.MeshBuilder.CreateGround("ground", {width: 12, height: 16});
    // add material to ground mesh for color, texture, lighting...
    let groundMaterial = new BABYLON.StandardMaterial("Ground Material");
    ground.material = groundMaterial;  // assign new material to ground mesh
    ground.material.diffuseColor = BABYLON.Color3.Red();  // set a basic color
    ground.receiveShadows = true;  // Allow shadows (shadow generator)

    // MeshBuilder Box (name, dimensions)
    const box = BABYLON.MeshBuilder.CreateBox("box", {height: 2, width: 2, depth: 2});
    // add material to box shape for color, texture, lighting...
    const boxMaterial = new BABYLON.StandardMaterial("material");
    boxMaterial.emissiveColor = new BABYLON.Color3(0.04, 0.04, 0.45);
    box.material = boxMaterial;
    box.position.y = 2.5;  // raise box 2.5 units above origin

    // Let there be light!  Emitted from everywhere and vector direction # affects range of shadow casting
    const light = new BABYLON.DirectionalLight("DirectionalLight", new BABYLON.Vector3(0, -10, 0));
    light.intensity = 0.5;  // Default is 1.

    // Shadow generator (caster and reciever required)
    const shadowGenerator = new BABYLON.ShadowGenerator(1024, light);  // quality, light source
    shadowGenerator.addShadowCaster(box);  // assign mesh to cast shadow

    // Code in this function will run ~60 times per second
    scene.registerBeforeRender(function () {
        box.rotation.x += 0.002;
        box.rotation.y += 0.005;
    });

    return scene;
};











