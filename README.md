yii-ajax-select-column
======================

Simple class for displaying CGridView columns as select elements which update the model via ajax

## Requirements:
- Only tested with Yii 1.1.13
- Requires jQuery 1.7+

## Installation:

1. Add the class file to protected/extensions/ajaxSelectColumn
2. Add the line below to your config file in the import section:
'application.extensions.ajaxSelectColumn.*',

## Usage:

    $this->widget('zii.widgets.grid.CGridView', array(
        'dataProvider'=>$dataProvider,
        'columns'=>array(
            ...
            array (
                'class' => 'AjaxSelectColumn',
                'header' => 'Category',
                'name' => 'catid',
                'url' => array('photo/updateColumn'),
            ),
            ...
        ),
    ));

When you change the selected value, four values will be posted to the URL:

1. id: the primary key of the model
2. class: the model's class, capitalized (ignore if not needed)
3. name: the name of the model's attribute
4. value: the new value of the input field

You can then use the values in your controller action to update the model. Here's an example of how you could do that, although purification is recommended:

    public function actionUpdateColumn()
    {
        $id = (int)$_POST['id'];
        $class = $_POST['class'];
        $name = $_POST['name'];
        $value = $_POST['value'];

        $model = $class::model()->findByPk($id);
        $p = new CHtmlPurifier();
        $model->{$name} = $p->purify($value);
        $model->update();
    }

## Screencap:
![Screencap](screencap.png)