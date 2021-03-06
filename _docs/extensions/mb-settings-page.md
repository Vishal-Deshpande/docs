---
title: MB Settings Page
---

{% include installation.html %}

After installing, you need to register a settings page and register meta boxes (and fields) for that settings page.

## Creating settings pages

Creating a settings page is done by filter `mb_settings_pages`. The code to add a settings page looks like this (this registers a theme options page under Appearance menu):

```php
add_filter( 'mb_settings_pages', 'prefix_options_page' );
function prefix_options_page( $settings_pages ) {
    $settings_pages[] = array(
        'id'          => 'theme-slug',
        'option_name' => 'theme_slug',
        'menu_title'  => 'Theme Options',
        'parent'      => 'themes.php',
    );
    return $settings_pages;
}
```

The filter takes an array of defined settings pages as the argument. The callback function must return an array of settings pages.

Each settings page is defined by an array of the following options:

Name|Description
---|---
`id`|Page ID. Required. Will be used as the slug in URL and option name (if `option_name` missed).
`option_name`|Option name where settings data is saved to. Optional. Takes `id` if missed. If you want to use theme mods, then set this to `theme_mods_$themeslug`.
`menu_title`|Menu title. Optional. Takes `page_title` if missed.
`page_title`|Page title. Optional. Takes `menu_title` if missed. Note: either `menu_title` or `page_title` (or both) must have value.
`capability`|Required capability to access the settings page. Optional. Default `edit_theme_options`.
`icon_url`|The icon for the menu. To use [a Dashicon](https://developer.wordpress.org/resource/dashicons/), set this to `dashicon-icon-name`. To use a SVG data image as a CSS background, set this to `data:image/svg+xml;base64...`. To use an image, set this to image URL. See `icon_url` parameter of [add_menu_page()](https://codex.wordpress.org/Function_Reference/add_menu_page) function.
`position`|Menu position. See `position` parameter of [add_menu_page()](https://codex.wordpress.org/Function_Reference/add_menu_page) function.
`parent`|ID of the parent page. Optional. Can be WordPress menu or custom settings page menu. See examples below for more details.
`submenu_title`|Set this to the default submenu title (first submenu) if the settings page is a top-level menu. Optional.
`help_tabs`|List of help tabs. Array in format `'tab-id' => 'Tab Content'`. Optional.
`style`|How do you want to style the settings page. Supports `boxes` which has same the style as normal WordPress meta boxes (like in the edit post screen) and `no-boxes` which has the same style as WordPress settings page. In `no-boxes` style, each meta box is a section of the settings page.
`columns`| The number of columns in the meta boxes. Can be 1 or 2. You might want to use 1 column with `no-boxes` style to match WordPress style.
`tabs`|Organized meta boxes and fields in tabs (see the example below). This param takes an array of (tab_id => Tab Title). Note: when using this param, you must specify which tab the meta box belongs by adding a new parameter `'tab' => tab_id`. See example below.
`submit_button`|The custom text for submit button. Optional.
`message`|The custom message displayed when saving options. Optional.


### Examples

This example registers a top-level menu pages and 2 sub settings page:

```php
add_filter( 'mb_settings_pages', 'prefix_settings_pages' );
function prefix_settings_pages( $settings_pages ) {
    $settings_pages[] = array(
        'id'            => 'my-options',
        'menu_title'    => 'Options',
        'option_name'   => 'my_options',
        'icon_url'      => 'dashicons-images-alt',
        'submenu_title' => 'Options', // Note this
    );
    $settings_pages[] = array(
        'id'          => 'my-options-im',
        'option_name' => 'my_options',
        'menu_title'  => 'Import',
        'parent'      => 'my-options', // Note this
    );
    $settings_pages[] = array(
        'id'          => 'my-options-ex',
        'option_name' => 'my_options',
        'menu_title'  => 'Export',
        'parent'      => 'my-options',
    );
    return $settings_pages;
}
```

Here is the result:

![registrer settings pages](https://i.imgur.com/5M3cwut.png)

Note that:

- The `submenu_title` allows us to set the first submenu text which can be different from the top-level menu text.
- The `parent` of submenus are set to the ID of top-level menu (`my-options`).

In case you want to add submenu to existing WordPress pages, set the `parent` option to:

Value|Page
---|---
`index.php`|Dashboard
`edit.php`|Posts
`upload.php`|Media
`link-manager.php`|Links
`edit.php?post_type=page`|Pages
`edit-comments.php`|Comments
`edit.php?post_type=your_post_type`|Custom post type
`themes.php`|Apperance
`plugins.php`|Plugins
`users.php`|Uses
`tools.php`|Tools
`options-general.php`|Settings

## Creating settings fields

Creating settings meta boxes and fields for settings pages is the same as for posts. You need to hook to `rwmb_meta_boxes` and set a param `settings_pages` to the settings page(s) you want to add to.

The sample code is below:

```php
add_filter( 'rwmb_meta_boxes', 'prefix_options_meta_boxes' );
function prefix_options_meta_boxes( $meta_boxes ) {
    $meta_boxes[] = array(
        'id'             => 'general',
        'title'          => 'General',
        'settings_pages' => 'theme-slug',
        'fields'         => array(
            array(
                'name' => 'Logo',
                'id'   => 'logo',
                'type' => 'file_input',
            ),
            array(
                'name'    => 'Layout',
                'id'      => 'layout',
                'type'    => 'image_select',
                'options' => array(
                    'sidebar-left'  => 'https://i.imgur.com/Y2sxQ2R.png',
                    'sidebar-right' => 'https://i.imgur.com/h7ONxhz.png',
                    'no-sidebar'    => 'https://i.imgur.com/m7oQKvk.png',
                ),
            ),
        ),
    );
    $meta_boxes[] = array(
        'id'             => 'colors',
        'title'          => 'Colors',
        'settings_pages' => 'theme-slug',
        'fields'         => array(
            array(
                'name' => 'Heading Color',
                'id'   => 'heading-color',
                'type' => 'color',
            ),
            array(
                'name' => 'Text Color',
                'id'   => 'text-color',
                'type' => 'color',
            ),
        ),
    );

    $meta_boxes[] = array(
        'id'             => 'info',
        'title'          => 'Theme Info',
        'context'        => 'side',
        'settings_pages' => 'theme-slug',
        'fields'         => array(
            array(
                'type' => 'custom_html',
                'std'  => 'A responsive theme for businesses and agencies.',
            )
        ),
    );
    return $meta_boxes;
}
```

Note:

- You must set `settings_pages` to the ID of the settings page. If you want to add meta boxes to 2 or more settings pages, set it to an array of settings pages' IDs
- All fields settings stay unchanged.
- The settings page uses 2 columns layout, which mimic WordPress post screen and the value for `context` has the same meaning.

## Using tabs

The example below registers 1 top level settings page with 3 tabs. Each tabs has one meta box. The style is set to `no-boxes` and `columns` is set to 1 to match WordPress style:

```php
// Register settings page. In this case, it's a theme options page
add_filter( 'mb_settings_pages', 'prefix_options_page' );
function prefix_options_page( $settings_pages ) {
    $settings_pages[] = array(
        'id'          => 'pencil',
        'option_name' => 'pencil',
        'menu_title'  => 'Pencil',
        'icon_url'    => 'dashicons-edit',
        'style'       => 'no-boxes',
        'columns'     => 1,
        'tabs'        => array(
            'general' => 'General Settings',
            'design'  => 'Design Customization',
            'faq'     => 'FAQ & Help',
        ),
        'position'    => 68,
    );
    return $settings_pages;
}

// Register meta boxes and fields for settings page
add_filter( 'rwmb_meta_boxes', 'prefix_options_meta_boxes' );
function prefix_options_meta_boxes( $meta_boxes ) {
    $meta_boxes[] = array(
        'id'             => 'general',
        'title'          => 'General',
        'settings_pages' => 'pencil',
        'tab'            => 'general',

        'fields' => array(
            array(
                'name' => 'Logo',
                'id'   => 'logo',
                'type' => 'file_input',
            ),
            array(
                'name'    => 'Layout',
                'id'      => 'layout',
                'type'    => 'image_select',
                'options' => array(
                    'sidebar-left'  => 'https://i.imgur.com/Y2sxQ2R.png',
                    'sidebar-right' => 'https://i.imgur.com/h7ONxhz.png',
                    'no-sidebar'    => 'https://i.imgur.com/m7oQKvk.png',
                ),
            ),
        ),
    );
    $meta_boxes[] = array(
        'id'             => 'colors',
        'title'          => 'Colors',
        'settings_pages' => 'pencil',
        'tab'            => 'design',

        'fields' => array(
            array(
                'name' => 'Heading Color',
                'id'   => 'heading-color',
                'type' => 'color',
            ),
            array(
                'name' => 'Text Color',
                'id'   => 'text-color',
                'type' => 'color',
            ),
        ),
    );

    $meta_boxes[] = array(
        'id'             => 'info',
        'title'          => 'Theme Info',
        'settings_pages' => 'pencil',
        'tab'            => 'faq',
        'fields'         => array(
            array(
                'type' => 'custom_html',
                'std'  => 'Having questions? Check out our documentation',
            ),
        ),
    );
    return $meta_boxes;
}
```

Here is the result:

![settings page with tabs](https://i.imgur.com/yb9Admk.png)

## Data

Settings values are saved in WordPress option as an array with the option name is `option_name` in the settings page configuration. The keys of that array are the field IDs and values are the field values.

## Getting field value

You're able to use helper function [rwmb_meta()](/rwmb-meta/) to get field value as follows:

```php
$value = rwmb_meta( $field_id, array( 'object_type' => 'setting' ), $option_name );
echo $value;
```

Notes:

- In the 2nd parameter, you need to pass `'object_type' => 'setting'`, and
- In the last parameter, you need to pass the option name for the settings page

Other parameters are the same as for posts. Please see [this documentation](/displaying-fields/) for details.

{% include alert.html type="warning" content="It requires the extension version 1.2+ to use the helper function. If you're using an older version, please [update now](/extensions/update/)." %}

In case you use an older version than 1.2, you can get the settings manually:

```php
$settings = get_option( 'option_name' );
$field_id = 'your_field_id';
if ( isset( $settings[$field_id] ) ) {
    return $settings[$field_id];
}
```

Note that this code returns only raw data of field value. It doesn't return meaningful information for images, file, etc. To do that, please add a small piece of code as follow:

```
// Getting images
$settings = get_option( 'option_name' );
$image_ids = $settings['images'];
foreach ( $image_ids as $image_id ) {
    $image = RWMB_Image_Field::file_info( $image_id, array( 'size' => 'thumbnail' ) );
    echo '<img src="' . $image['url'] . '">';
}
```

{% include helpers.html %}