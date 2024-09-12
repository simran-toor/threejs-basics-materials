## 12 MATERIALS 
* materials are used to put a colour on each visible pixel of the geometries 
* algorithms are written in programs called shaders
* don't need to writes shaders b/c can use built-in materials 
## PREPARE OUR SCENE 
* create 3  `Meshes` composed of 3 different geometries 
    - `sphere`
    - `plane`
    - `torus` 
* use same `MeshBasicMaterial` on all 3:
    ```
    /** 
    * Objects
    */
    const material = new THREE.MeshBasicMaterial()

    const sphere = new THREE.Mesh(
        new THREE.SphereGeometry(0.5, 16, 16),
        material
    )
    sphere.position.x = - 1.5

    const plane = new THREE.Mesh(
        new THREE.PlaneGeometry(1, 1),
        material
    )

    const torus = new THREE.Mesh(
        new THREE.TorusGeometry(0.3, 0.2, 16, 32),
        material
    )
    torus.position.x = 1.5 

    scene.add(sphere, plane, torus)
    ```
* rotate the objects in the `tick` function:
    ```
    // Update objects 
    sphere.rotation.y = 0.1 * elapsedTime
    plane.rotation.y = 0.1 * elapsedTime
    torus.rotation.y = 0.1 * elapsedTime

    sphere.rotation.x = 0.15 * elapsedTime
    plane.rotation.x = 0.15 * elapsedTime
    torus.rotation.x = 0.15 * elapsedTime
    ```
* load all the door texture with a `TextureLoader` (at the top):
    ```
    /**
    *  Textures
    */
    const textureLoader = new THREE.TextureLoader()

    const doorColorTexture = textureLoader.load('/textures/door/color.jpg')
    const doorAlphaTexture = textureLoader.load('/textures/door/alpha.jpg')
    const doorAmbientOcclusionTexture = textureLoader.load('/textures/door/ambientOcclusion.jpg')
    const doorHeightTexture = textureLoader.load('/textures/door/height.jpg')
    const doorNormalTexture = textureLoader.load('/textures/door/normal.jpg')
    const doorMetalnessTexture = textureLoader.load('/textures/door/metalness.jpg')
    const doorRoughnessTexture = textureLoader.load('/textures/door/roughness.jpg')
    const matcapTexture = textureLoader.load('/textures/matcaps/1.png')
    const gradientTexture = textureLoader.load('/textures/gradients/3.jpg')
    ```
* until now we have been using `MeshbasicMaterial` which applies a uniform colour or a texture on our geometry
* search for "material" on `Three.js documentation` 
* test lots of them 
## MESH BASIC MATERIAL 
* `MeshBasicMaterial` is the most basic material 
* most of materials properties can be set in 2 ways:
    ```
    // Directly while instanciating
    const material = new THREE.MeshBasicMaterial({
        map: doorColorTexture
    })

    // After instantiating
    const material = new THREE.MeshBasicMater()
    material.map = doorColorTexture
    ```
* `color` will apply a uniform color on the surface of the geometry 
* once instantitated, the `color` property becomes `Color`:
    ```
    material.color = new THREE.Color('#f0f')
    ```
* can combine `color` and `map`:
    ```
    material.map = doorColorTexture
    material.color = new THREE.Color('#ff0000')
    ```
* `wireframe` will show the triangles that compose the geometry: 
    ```
    material.wireframe = true
    ```
* `opacity` controls the general opacity 
* need to set `transparent = true`:
    ```
    material.transparent = true 
    material.opacity = 0.5 
    ```
* `alphaMap` controls the transparency with a texture:
    ``` 
    material.transparent = true 
    material.alphaMap = doorAlphaMap
    ```
* `side` lets you decide which side of a face is visible:
    - `THREE.FrontSide` (default)
    - `THREE.BackSide`
    - `THREE.DoubleSide` 
* `DoubleSide` uses a lot of resources (avoid using if possible)
* some of these properties like `wireframe` or `opacity` can be used with other types of materials 
## MESH NORMAL MATERIAL 
* `MeshNormalMaterial` displays a purple colour: 
    ```
    const material = new THREE.MeshNormalMaterial()
    ```
* normals are information that contain the direction of the face 
* normals can be used to lighting, reflection, refraction, etc. 
* `MeshNormalMaterial` shares common properties with `MeshBasicMaterial` like:
    - `wireframe`
    - `transparent`
    - `opacity`
    - `side` 
* but there is also a `flatShading` property 
    ```
    material.flatShading = true
    ```
* `flatShading` will flatten the faces, meaning that the normals won't be interpolated between the vertices
* `MeshNormalMaterial` is usually used to debug normals, but the colour looks great so it can be used for your projects
    * https://www.ilithya.rocks
## MESHMATCAPMATERIAL 
* `MeshMatcapMaterial will display a colour by using the normals as a reference to pick the right colour on a texture that looks like a sphere
* set the matcap texture with the `matcap` property: 
    ```
    const material = new THREE.MeshMatcapMaterial()
    material.matcap = matcapTexture
    ```
* it creates an illusion that the objects are being illuminated (without needing to add lights)
* try different textures available in the `/static/textures/matcaps/` folder
* can find matcap textures at: `https://github.com/nidorx/matcaps` (make sure you have rights if not for personal use)
* can create you own with 3D software, need to put camera infront of sphere, lights around, material on sphere and do a square render
* or create it in a 2D like photoshop
## MESH DEPTH MATERIAL 
* `MeshDepthMateria` will simply colour the geometry in white, if it's close to the `near` and in black if it's close to the `far` value of the camera:
    ```
    const material = new THREE.MeshDepthMaterial()
    ```
## ADDING A FEW LIGHTS
* create an `AmbientLight`:
    ```
    const ambientLight = new THREE.AmbientLight(0xffffff, 0.5)
    scene.add(ambientLight)
    ```
* create a `PointLight`:
    ```
    const pointLight = new THREE.PointLight(0xffffff, 0.5)
    pointLight.position.x = 2
    pointLight.position.y = 3 
    pointLight.position.z = 4
    scene.add(pointLight)
    ```
## MESH LAMBERT MATERIAL
* `MeshLambertMaterial` reacts to light:
    ```
    const material = new THREE.MeshLambertMaterial()
    ```
* it is performant but we can see strange patterns on the geometry 
## MESH PHONG MATERIAL 
* `MeshPhongMaterial` is similar to `MeshLambertMaterial` but the strang patterns are less visible and you can also see the light reflection:
    ```
    const material = new THREE.MeshPhongMaterial()
    ```
* can control the light reflection with `shininess` and the colour of the reflection with `specular`:
    ```
    const material = new THREE.MeshPhongMaterial()
    material.shininess = 100 
    material.specular = new THREE.Color(0x1188ff)
    ``` 
## MESH TOON MATERIAL 
* `MeshToonMaterial` similar to `MeshLambertMaterial` but more cartoonish:
    ```
    const material = new THREE.MeshToonMaterial()
    ```
* can add more steps to the colouration using `gradientMap` property and using `gradientTexture`:
    ```
    const material = new THREE.MeshToonMaterial()
    material.gradientMap = gradientTexture
    ```
* we see a gradient instead of a clear seperation b/c the gradient is smalla nd the `magFilter` tries to fix it with `mipmapping`
* set the `minFilter` to `THREE.NearestFilter`
* also deactivate the mipmapping with `gradientTexture.generateMipmaps = false`
* do this under textures:
    ```
    gradientTexture.minFilter = THREE.NearestFilter
    gradientTexture.magFilter = THREE.NearestFilter
    gradientTexture.generateMipmaps = false
    ```
## MESH STANDARD MATERIAL
* `MeshStandardMaterial` uses physically based rendering principles (PBR)
* Like `MeshLambertMaterial` and `MeshPhongMaterial`, it supports lights but with a more realistic algorithm and better parameters like `roughness` and `metalness`
    ```
    const material = new THREE.MeshStandardMaterial()
    ```
* change `roughness` and `metalness`:
    ```
    material.metalness = 0.45 
    material.roughness = 0.65
    ```
## ADD A DEBUG UI
* adding it from scratch 
* in terminal, on project folder run `npm install --save dat.gui`
* import `dat.gui` at top of page
    ```
    import * as dat from 'dat.gui'
    ```
* instantiate:
    ```
    /**
    * Debug
    */
    const gui = new dat.GUI()
    ```
* add the controls
* need to add them after material (or whatever the control is for)
    ```
    gui.add(material, 'metalness').min(0).max(1).step(0.0001)
    gui.add(material, 'roughness').min(0).max(1).step(0.0001)
    ```
* `map` allows you to apply a texture:
    ```
    material.map = doorColorTexture
    ```
* `aoMap`("ambient occlusion map") will add shadows where the texture is dark, must ad a second set of UV (coordinates that tell how the texture is supposed to be applied on the geometry) names `uv2`
* in this case, it's the same coordinates as the default UV so we are going to reuse them:
* add coordinates:
    ```
    sphere.geometry.setAttribute('uv2', new THREE.BufferAttribute(sphere.geometry.attributes.uv.array, 2))
    plane.geometry.setAttribute('uv2', new THREE.BufferAttribute(plane.geometry.attributes.uv.array, 2))
    torus.geometry.setAttribute('uv2', new THREE.BufferAttribute(torus.geometry.attributes.uv.array, 2))

    ```
* add `aoMap`:
    ```
    material.aoMap = doorAmbientOcclusionTexture 
    ```
* change intensity:
    ```
    material.aoMapIntensity = 1
    ```
* add to `dat.gui`:
    ```
    gui.add(material, 'aoMapIntensity').min(0).max(10).step(0.0001)
    ```
* `displacementMap` will move the vertices to create relief:
    ```
    material.displacementMap = doorHeightTexture
    ```
* currently look terrible because it lacks vertices and the displacement is too strong
* add subdivisions to each shape:
    ```
    new THREE.SphereGeometry(0.5, 64, 64)

    new THREE.PlaneGeometry(1, 1, 100, 100)

    new THREE.TorusGeometry(0.3, 0.2, 64, 128)
    ```
* reduce displacement (height):
    ``` 
    material.displacementScale = 0.05
    ```
* add displacment scale to debug UI:
    ```
    gui.add(material, 'displacementScale').min(0).max(1).step(0.0001)
    ```
* instead of specifying uniform `metalness` and `roughness` for the whole geometry, use `metalnessMap` and `roughnessMap`:
    ```
    material.metalnessMap = doorMetalnessTexture
    material.roughnessMap = doorRoughnessTexture
    ```
* reflection still looks weird b/c `metalness` and `roughness` properties still affect each map respectively, either comment them out or use their original values:
    ```
    material.metalness = 0 
    material.roughness = 1
    ```
* `normalMap` fakes the normals orientation and adds details on the surface regardless of the subdivision:
    ```
    material.normapMap = doorNormalMaterial
    ```
* can change the normal intensity with the `normalScale` property (Vector2):
    ```
    material.normalScale.set = (0.1, 0.1)
    ```
* control the alpha using `alphaMap` property:
    ```
    material.transparent = true
    material.alphaMap = doorAlphaTexture
    ```
## MESH PHYSICAL MATERIAL
* `MeshPhysicalMaterial` is the same as `MeshStandardMaterial` supports a clear coat effect
## POINTS MATERIAL
* can use `PointsMaterial` with particles 
## SHADER MATERIAL AND RAW SHADER MATERIAL
* `ShaderMaterial` and `RawShaderMaterial` can both be used to crate your own materials 
## EVIRONMENT MAP
* it's an image of what's surrounding the scene
* can be used for reflection or refraction but also for general lighting
* they are supported by multiple materials but we're going to use `MeshStandardMaterial`
* set a nice looking `MeshStandardMaterial`:
    ```
    const material = new THREE.MeshStandardMaterial()
    material.metalness = 0.7
    material.roughness = 0.2
    ```
* Three.js only supports cube environment maps 
* can find multiple maps in the `/static/textures/environmentMaps/` folder
* to load a cube texture, must use the `CubeTextureLoader` instead of `TextureLoader`
* instantiate the `CubeTextureLoader` before instantiating the `material` and call its `load(...)` method but use an array of paths:
    ```
    const cubeTextureLoader = new THREE.CubeTextureLoader()
    const environmentMapTexture = cubeTextureLoader.load([
        '/textures/environmentMaps/0/px.jpg',
        '/textures/environmentMaps/0/nx.jpg',
        '/textures/environmentMaps/0/py.jpg',
        '/textures/environmentMaps/0/ny.jpg',
        '/textures/environmentMaps/0/pz.jpg',
        '/textures/environmentMaps/0/nz.jpg',
    ])
    ```
* have to load images in order:
    1. `px` - positive x
    2. `nx` - negative x
    3. `py` - positive y
    4. `ny` - negative y
    5. `pz` - positive z
    6. `nz` - negative z
* use the `environmentMapTexture` in the `envMap` property of the material:
    ```
    material.envMap = environmentMapTexture
    ```
* can tweak `metalness` and `roughness` for different results and test other environment maps for the foler 
## WHERE TO FIND ENVIRONMENT MAPS
* search the web 
* make sure you have rights if not for personal use 
* `HDRIHaven`:
    - has hundreds of great HDRI's (High Dynamic Range Imaging)
    - not cube maps
    - `CC0 license` - can do anything you want without having to credit the authors 
* to convert HDRIs to cube maps, use this online tool: `https://matheowis.github.io/HDRI-to-CubeMap/`

    
