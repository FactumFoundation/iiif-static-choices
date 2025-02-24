iiif-static-choices
===

A IIIF static tile and manifest generator built using Python to generate IIIF tiled images and manifests.

This application was put together to de-mystify the process of creating and hosting IIIF content and allow implementations
without the need for specialist infrastructure such as imaging servers (iip) and manifest servers.

It also includes the Bodleian Libraries Mirador plug-in written as part of the ARCHiOx project and the Mirador image-tools
plug-in and is also intended as a vehicle to test the capabilities of using 2D images to encode and present 3D information.

A simple Python server application is included to serve the Mirador or Openseadragon via your local browser, it is not
intended for a production environment and would need replacing with something else more suitable for the job.

Licensing
===

This project is covered by the [Mozilla Public License](LICENSE) except for the bundled [Mirador](https://github.com/ProjectMirador/mirador?tab=Apache-2.0-1-ov-file#readme) and [Openseadragon](https://github.com/openseadragon/openseadragon?tab=BSD-3-Clause-1-ov-file#readme) builds
which are covered by their own licenses.  

The tile generation part of this project is based on and translated from the work of Glen Robson in his Java [iiif-tiler](https://github.com/glenrobson/iiif-tiler)

Dependencies
===

- Python 3.9
- Poetry

Installation Instructions
===

Install the project after installing Python and Poetry, as follows:

```bash
poetry install --no-root
```

Basic Instructions
===

For this quick guide we'll be using the existing example images and manifest-config.yml files present in the `image` directory.

1. Run the tile generation as follows, this will generate v3 static tiles in the `iiif/image` folder:

    ```bash
    poetry run python iiif_generator.py tiles -t 256 -v 3.0
    ```
2. Run the manifest generator as follows, this will generate a v3 manifest in the `iiif/manifest` folder:

    ```bash
    poetry run python iiif_generator.py manifest -f ammonite-config.yml -o pyritised-ammonite.json 
    ```

3. Run the basic server application as follows:

    ```bash
    poetry run python server.py 8000
    ```

Using your own images and manifest-config.yml
===

To use your own images and manifest-config.yml do the following:

1. Copy your image files into the project directory called `image`.

2. Create a manifest_config.yml file from the template provided, add real values to this and copy this to the `image` directory too.  You can find a populated one in `image` for reference. Or skip ahead and use the examples already there.

3. Run through the steps in [Basic Instructions](#basic-instructions) again.

What should it look like?
===

If you've done the above set up correctly and the server is running, you can go to your browser and enter the address: http://0.0.0.0:8000/ and you will see the following.  Click the buttons like the cursor does in the animated gif to play around in 2.5D in Mirador.

![Animated picture showing a pyritised ammonite and someone interacting with it using the ARCHiOx Mirador plug-in](examples/fossil.gif)

Todo
===

- add in thumbnail generation for the choices layers in Mirador, this could be done during manifest generation  
- add in logo generation
- add in multi-page manifests
- add some unit tests to prevent development breaking



Advanced instructions
===

### IIIF Generator: `tiles` Command Parameters

The `iiif_generator.py tiles` command is used to generate IIIF image tiles from a directory of input images. Below is a detailed description of all available parameters:

#### **Available Parameters**

##### 1. `-i`, `--identifier`
- Type: `str`
- Default Value: `http://0.0.0.0:8000/iiif/`
- Description: Sets the identifier in the `info.json` file generated for each IIIF image. This identifier serves as the base URL for accessing the generated tiles.

##### 2. `-z`, `--zoom_levels`
- Type: `int`
- Default Value: `5`
- Description: Specifies the number of zoom levels generated for each image. A higher value results in more detailed zoom levels, increasing storage requirements.

##### 3. `-v`, `--version`
- Type: `str`
- Default Value: `2.1.1`
- Options: `2.1.1` or `3.0`
- Description: Defines the IIIF image API version used for generating tiles. Version `3.0` follows the latest IIIF specifications, while `2.1.1` is a widely used stable version.

##### 4. `-t`, `--tile_size`
- Type: `int`
- Default Value: `1024`
- Description: Specifies the tile size in pixels. The images will be divided into square tiles of this dimension.

##### 5. `-o`, `--output`
- Type: `str`
- Default Value: `iiif`
- Description: Defines the directory where the generated IIIF image tiles will be stored. By default, they are saved in the `iiif/` directory.

##### 6. `-d`, `--input_directory`
- Type: `str`
- Default Value: `image`
- Description: Specifies the input directory where the original images to be tiled are located. Supported image formats include `.png`, `.jpg`, and `.webp`.

---

#### **Usage Example**

``` bash
poetry run python iiif_generator.py tiles -i http://127.0.0.1:8000/iiif/ -z 5 -v 3.0 -t 256 -o iiif -d image
```

### IIIF Generator: `manifest` Command Parameters

The `iiif_generator.py manifest` command is used to generate a **IIIF manifest** from a directory of images and a YAML configuration file. Below is a detailed description of all available parameters:

#### **Available Parameters**

##### 1. `-o`, `--output`
- Type: `str`
- Default Value: `manifest.json`
- Description: Specifies the output file name for the generated IIIF manifest. By default, the manifest is saved as `/iiif/manifest/manifest.json`.

##### 2. `-d`, `--input_directory`
- Type: `str`
- Default Value: `image`
- Description: Defines the directory where the images to be included in the manifest are located. 

##### 3. `-f`, `--file_name`
- Type: `str`
- Required: `True`
- Description: Specifies the **YAML configuration file** that contains metadata and structure details for the manifest. This file is essential for generating a valid IIIF manifest.

##### 4. `-s`, `--host_name`
- Type: `str`
- Default Value: `http://0.0.0.0:8000`
- Description: Defines the base **host URL** that will be used in all URIs within the generated manifest.

---

#### **Usage Example**
``` bash
poetry run python iiif_generator.py manifest -o pyritised-ammonite.json -d image -f ammonite-config.yml  -s http://127.0.0.1:8000 
```

IIIF REFERENCES AND EXAMPLES (test)
===
- [Introduction to IIIF](https://training.iiif.io/intro-to-iiif/)
- [Documentation and workshop materials for IIIF training](https://training.iiif.io/)
- [Online example on Factum's server](https://highres.factumfoundation.xyz/iiif/index.html)

