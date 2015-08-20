Uploadable-Behavior
==================

With the Uploadable-behavior you are able to upload files easily.

[doc_toc]

Loading
-------
You can load the behavior in your model via:

    $this->addBehavior('Utils.Uploadable', []);

Usage
-----

Imagine you want to make the feature an user is able to add an image as avatar.

First of all you need a new column in your table. Let's call it `avatar`.

Now you need to let our behavior know that you want to make the field `avatar` an upload-field. 
You have to do that when you add the behavior to the model:

    $this->addBehavior('Utils.Uploadable', [
        'avatar',
    ]);

Also we have to customize our view:

    // add the type to the create-method
    echo $this->Form->create($entity, ['type' => 'file']);
    
    // add the avatar-input
    echo $this->Form->input('avatar', ['type' => 'file']);

Now we are done. you are ready to uplaod your file. In the next section all configurations under your field will be described.

Configurations
--------------
There are multiple configurations for the behavior available. Note that configurations will be done under a regsitered field.

Example:

The empty array of `avatar` will contain the configurations

### Fields
The `fields` configuration contains an array with avaliable columns you want to set:

- `directory` - this is the column who will contain the path to your file, without the file-name in it. For example:
`uploads\blogs\4\`
- `type` - this column will contain the type of the uploaded file. For example: `image/png`.
- `size` - this column contains the size of the uploaded file. For example: `72896` (kb).
- `fileName` - this column contains the name of the file. For example: `myFile.png`.
- `filePath` - this column contains the path to the file, including the file name. For example: 
`uploads\blogs\4\myFile.png`. This column is automatically used for the given column to upload. In our example above, in
the column `avatar` the string `uploads\blogs\4\myFile.png` is saved automatically.

Example:

    $this->addBehavior('Utils.Uploadable', [
        'avatar' => [
          'fields' => [
            'directory' => 'avatar_directory',
            'url' => 'avatar_url',
            'type' => 'avatar_type',
            'size' => 'avatar_size',
            'fileName' => 'avatar_name',
            'filePath' => 'avatar'
          ]
        ],
    ]);

In this case:
- the directory will be stored in the `avatar_directory` column,
- the url will be stored in the column `avatar_url`,
- the type will be stored in the column `avatar_type`,
- the size of the file will be stored in the column `avatar_size`,
- the fileName will be stored in the column `avatar_name`,
- the filePath (including the file name) will be stored in the column `avatar`,

### Remove File On Update

The `removeFileOnUpdate` configuration is a boolean you can set if you want to remove all previous files on a new uploaded file.
So imagine I will upload an avatar for the second time. The previous avatar would be deleted if you set `removeFileOnUpdate` to `true`.

Example:

    $this->addBehavior('Utils.Uploadable', [
      'avatar' => [
        'removeFileOnUpdate' => true
        ],
      ]
    ]);

> Note: When the uploaded file has the same name (and extension) as the previous file, it will be overridden.

### Remove File On Delete

The `removeFileOnDelete` configuration is a boolean you can set if you want to remove all files when delete the linked entity.
So, looking to the previous example: When I remove my account, my avatar(s) would be deleted when `removeFileOnDelete` is set to `true`.

Example:

    $this->addBehavior('Utils.Uploadable', [
      'avatar' => [
        'removeFileOnDelete' => true
        ],
      ]
    ]);

### Path

The `path` configuration contains the path to write the file to. Default this configuration looks like:

    ```
    {ROOT}{DS}{WEBROOT}{DS}uploads{DS}{model}{DS}{field}{DS}
    ```

This path would be converted to: `webroot/uploads/Users/5/` (asuming our user-id is `5`.

The following templates are avaiable:

- `{ROOT}` - is the defined ROOT-variable 
- `{WEBROOT}` - is the defined WEBROOT-variable
- `{field}` - is the chosen field (read docs later on)
- `{model}` - is the current model (like Users / Uploads / Bookmarks)
- `{DS}` - Directory Seperator
- `//` - Directory Seperator
- `/` - Directory Seperator
- `\\` - Directory Seperator

Example:

    $this->addBehavior('Utils.Uploadable', [
      'avatar' => [
        'path' => '{ROOT}{DS}{WEBROOT}{DS}custom{DS}directory{DS}{model}{DS}{field}{DS}'
        ],
      ]
    ]);


### Field

The `field` configuration is the field of the entity to use. Default we use the `field` in our path to create a folder per `id`.

Example:

    $this->addBehavior('Utils.Uploadable', [
      'avatar' => [
        'field' => 'username',
        'path' => '{ROOT}{DS}{WEBROOT}{DS}uploads{DS}{model}{DS}{field}{DS}'
        ],
      ]
    ]);
    
In this example the following path would be generated: `webroot/uploads/Users/myusername/`


### Filename

With the `filename` configuration you are able to change the default filename of the user. Default the value is `{ORIGINAL}`.

The following templates are avaiable:

- `{ORIGINAL}` - is the defined ROOT-variable 
- `{field}` - is the chosen field of the entity
- `{extension}` - is the extension of the uploaded file

In this case we would be able to change the filename to a field of the entity:

    $this->addBehavior('Utils.Uploadable', [
      'avatar' => [
        'field' => 'id',
        'path' => '{ROOT}{DS}{WEBROOT}{DS}uploads{DS}{model}{DS}',
        'fileName' => '{field}.{extension}'
        ],
      ]
    ]);

This example will place all avatars of all users in the path `webroot/uploads/Users/`, and saves the file as `5.png` (example).


To Do
-----

The following features will be added soon:

- Validation methods
