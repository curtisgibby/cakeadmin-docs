Adding PostTypes
================

In this short tutorial we will explain how to register your PostTypes and customize them to your needs.

[doc_toc]

Models
------

Each model can be used as PosType. The only thing you need to do is to register it via the `Configure` class of CakePHP.
This example will show you how to register your model:

    Configure::write('CA.Models.blogs', 'Blogs');
    
The `blogs` is the name of your Model in lower case. `Blogs` is the Model to use. When you want to register a Model of a
plugin, use `Utils.Blogs` for example.

When you will check your admin-panel you will see a new menu-item called to your model. When clicking on it you are able
to create, read, update and delete records of your model.

Custom configurations
---------------------

After you have seen your PostType, maybe you want to change it's configurations. This can easily be done by adding them
to your model this way:

    // as a property:
    public $postType = [];
    
    // as a function:
    public function postType() {
        return [];
    }
    
The following configurations can be done:

- `model` - The model to use. Default set to the chosen model.
- `menu` - Boolean if the PostType should be added to the menu. Default `true`.
- `menuWeight` - Integer of the height of the item in the whole menu.
- `slug` - Slug of the PostType. Automatically generated.
- `name` - Name of the PostType. Automatically generated.
- `alias` - Alias (humanized name) of the PostType. Automatically generated.
- `aliasLc` - Same as above, but lower-cased.
- `singularAlias` - Alias (humanized name, in singular form) of the PostType. Automatically generated.
- `singularAliasLc` - Same as above, but lower-cased.
- `description` - Description of the PostType. Default `null`.
- `actions` - Array with actions as key and bool as value to enable or disable actions. Default all actions are enabled.
- `filters` - Array of filters to use (SearchComponent of the Utils-plugin).
- `query` - Custom query-function to customize your queries.
- `tableColumns` - Array of the columns of the table to use.
- `formFields` - Array of the fields of the form to use.

> Note: Want to read more about register PostTypes? Go to [this section](/docs/cakeadmin/1.0/components/posttypes).

TableColumns
------------

Automatically, some columns are selected to show on the 'index'-action of your PostType. You can configure your own 
selection by changing the `tableColumns`-key of the PostTypes configs. Look at this example:

    public function postType() {
        return [
            'tableColumns' => [
                'id',
                'name',
                'category_id' => [
                    'get' => 'category.name'
                ],
                'created_by',
                'modified_by',
            ]
        ];
    }
        
This is your own selection of columns to show on your 'index'-action.

The `get`-key will be explained [here](#queries-and-getters).

FormFields
----------

Automatically, some fields are selected to show on the `add`- and `edit`-action of your PostType. You can configure your
own selection by changing the `formFields`-key of the PostTypes configs. Look at this example:

    public function postType() {
        return [
            'formFields' => [
                'id',
                'name' => [
                    'type' => 'input',
                    'error' => 'Failure!'
                ],
                'country' => [
                    'default' => 'The Netherlands'
                ],
                'created_by' => [
                    'label' => 'creator'
                ],
                'modified_by',
            ]
        ];
    }
    
This is a small example about how to use options for the form-inputs. Refer to this docs of CakePHP to check all types
of configs: http://book.cakephp.org/3.0/en/views/helpers/form.html#options.

Associations
------------

Sometimes you need a drop-down with a list to choose one. This is automatically generated by the `PostTypesComponent` by
adding your relations well in your model. Check out this docs about associations: 
http://book.cakephp.org/3.0/en/orm/associations.html.

Queries and Getters
-------------------

Sometimes you want to manipulate your queries. This can be done by using the `query`-key. You have to give it a function
wich gets a `Query`-object, and returns a `Query`-object.

    public function postType() {
        return [
            'query' => function($query) {
                $query->contain('Comments');
                return $query;
            }
        ];
    }
    
When your result-item will have contained data, you can use the `get`-key:

    public function postType() {
        return [
            'tableColumns' => [
                'id',
                'name',
                'category_id' => [
                    'get' => 'category.name'
                ]
                'created_by',
                'modified_by',
            ]
        ];
    }
    
By using this key, you won't see an related primary key, but the wanted value.

Events
------

The following events are available for every PostType:
- `Controller.PostTypes.*PostTypeName*.beforeIndex` - Fired before the action `index` has been fired.
- `Controller.PostTypes.*PostTypeName*.afterIndex` - Fired after the action `index` has been fired.
- `Controller.PostTypes.*PostTypeName*.beforeAdd` - Fired before the action `add` has been fired.
- `Controller.PostTypes.*PostTypeName*.afterAdd` - Fired after the action `add` has been fired.
- `Controller.PostTypes.*PostTypeName*.beforeEdit` - Fired before the action `edit` has been fired. The variable `id`
has been given as second parameter.
- `Controller.PostTypes.*PostTypeName*.afterEdit` - Fired after the action `edit` has been fired. The variable `id`
has been given as second parameter.
- `Controller.PostTypes.*PostTypeName*.beforeView` - Fired before the action `view` has been fired. The variable `id`
has been given as second parameter.
- `Controller.PostTypes.*PostTypeName*.afterView` - Fired after the action `view` has been fired. The variable `id`
has been given as second parameter.
- `Controller.PostTypes.*PostTypeName*.beforeDelete` - Fired before the action `delete` has been fired. The variable `id`
has been given as second parameter.
- `Controller.PostTypes.*PostTypeName*.afterDelete` - Fired after the action `delete` has been fired. The variable `id`
has been given as second parameter.

