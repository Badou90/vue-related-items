# Eloquent vue related items control

use this snippet to create item

```` 
<related-table id="equipment" :columns="" :items="" create-url="" delete-url="" entry-id=""></related-table>
````

## Controllers

Your controller should have 2 actions: for creating and removing related items, and creating action should return JSON object with `id` field of created item in response object

## Columns

Columns array must be like 
````
[
    column_name => column_name_translation
]
````