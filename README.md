Better Font Awesome Library for WordPress
===========================

*The easiest way to integrate Font Awesome into your WordPress project.*

## Table of contents ##
1. [Introduction](https://github.com/MickeyKay/better-font-awesome-library#introduction)
1. [Features](https://github.com/MickeyKay/better-font-awesome-library#features)
1. [Usage](https://github.com/MickeyKay/better-font-awesome-library#usage)
1. [Initialization Parameters](https://github.com/MickeyKay/better-font-awesome-library#initialization-parameters)
1. [The Better Font Awesome Library Object](https://github.com/MickeyKay/better-font-awesome-library#the-better-font-awesome-library-object)
1. [Filters](https://github.com/MickeyKay/better-font-awesome-library#filters)
1. [To Do](https://github.com/MickeyKay/better-font-awesome-library#to-do)

## Introduction ##
The Better Font Awesome Library allows you to integrate any version of Font Awesome into your project (including the latest), along with accompanying CSS, a list of available icons, and a TinyMCE drop-down and shortcode generator.

## Features ##
* Integrates any version of Font Awesome, including the option to always use most recent version (no more manual updates!).
* Generates an easy-to-use [PHP object](#the-better-font-awesome-library-object) that contains all relevant info including: version, CSS URL, icons available in this version, prefix used (`icon` or `fa`).
* CDN speeds - Font Awesome CSS is pulled from [jsDelivr CDN](http://www.jsdelivr.com/#!fontawesome).
* Includes optional TinyMCE plugin with drop-down shortcode generator.
* Ability to choose between minified or unminified CSS.

## Usage ##
1. Copy the better-font-awesome-library folder into your project.

2. Add the following code to your main plugin file or your theme's function.php file.
   ```
	// Intialize Font Awesome once plugins are loaded
	add_action( 'plugins_loaded', 'my_prefix_load_bfa' );
	function my_prefix_load_bfa() {

		// Include the library file - modify the require_once path to match your directory structure
		require_once ( dirname( __FILE__ ) . '/better-font-awesome-library/better-font-awesome-library.php' );

		// Settings to initialize the Better Font Awesome Library object (defaults shown)
		$args = array(
				'version' => 'latest',
				'minified' => true,
				'remove_existing_fa' => false,
				'load_styles'             => true,
				'load_admin_styles'       => true,
				'load_shortcode'          => true,
				'load_tinymce_plugin'     => true,
		);
		
		// Initialize Font Awesome, or get the existing instance
		Better_Font_Awesome_Library::get_instance( $args );
	}
```

3. If desired, use the [Better Font Awesome Library object](#the-better-font-awesome-library-object) to manually include Font Awesome CSS, output lists of available icons, create your own shortcodes, and much more.

### Usage Notes ###
The Better Font Awesome Library is designed to work in conjunction with the [Better Font Awesome](https://wordpress.org/plugins/better-font-awesome/) WordPress plugin. The plugin initializes the library with its initialization args on the `plugins_loaded` hook, priority `5`. When using the Better Font Awesome Library in your project, you have two options:
1. Initialize later, to ensure that any Better Font Awesome plugin settings override yours.
2. Initialize earlier, to "take over" and prevent Better Font Awesome settings from having an effect.

## Initialization Parameters ($args) ##
The following parameters can be passed to `Better_Font_Awesome_Library::get_instance( $args )` in the `$args` array.

#### version ####
(string) Which version of Font Awesome you want to use.
* `'latest'` (default) - always use the latest available version.
* `'3.2.1'` - any existing Font Awesome version number.

#### minified ####
(boolean) Use minified Font Awesome CSS.
* `true` (default)
* `false` - uses unminified CSS.

#### remove_existing_fa ####
(boolean) Attempts to remove existing Font Awesome styles and shortcodes. This can be useful to prevent conflicts with other themes/plugins, but is no guarantee..
* `false` (default)
* `true`

#### load_styles ####
(boolean) Automatically loads Font Awesome CSS in the **front-end** of your site using `wp_enqueue_sripts()`.
* `true` (default)
* `false` - use this if you don't want to load the Font Awesome CSS in the front-end, or wish to do it yourself.

#### load_admin_styles ####
(boolean) Automatically loads Font Awesome CSS in the **admin** of your site using `admin_enqueue_scripts()`.
* `true` (default)
* `false` - use this if you don't want to load the Font Awesome CSS in the admin, or wish to do it yourself.

#### load_shortcode ####
(boolean) Loads the included `[icon]` shortcode.
* `false` (default)
* `true`

The shortcode looks like:
```
[icon name="" class="" unprefixed_class=""]
```

**name**  
Unprefixed icon name (e.g. star)

**class**  
Unprefixed [Font Awesome icon classes](http://fortawesome.github.io/Font-Awesome/examples/), to which the appropriate prefix will automatically be added (e.g. 2x spin)

**unprefixed_class**  
Classes that you wish to remain unprefixed (e.g. my-custom-class)

#### load_tinymce_plugin ####
(boolean) Loads a TinyMCE drop-down list of available icons (based on `version`), which generates a `[icon]` shortcode.
* `false` (default)
* `true`

## The Better Font Awesome Library Object ##
The Better Font Awesome Library object can be accessed with the following code:  
`Better_Font_Awesome_Library::get_instance();`

The object has the following public methods:
#### get_version() ####
(string) Returns the active version of Font Awesome being used.

#### get_stylesheet_url() ####
(string) Returns the Font Awesome stylesheet URL.

#### get_icons() ####
(array) Returns an alphabetical array of unprefixed icon names in the active version of Font Awesome.

#### get_prefix() ####
(string) Returns the version-dependent prefix ('fa' or 'icon') that is used in the icons' CSS classes.

#### get_api_data() ####
(object) Returns version data for the remote jsDelivr CDN (uses jsDelivr API). Includes all available versions and latest version.

#### Example: ####
```
// Get the Better Font Awesome Library object.
$my_bfa = Better_Font_Awesome_Library::get_instance( $args );

// Get info on the Font Awesome version being used.
$version = $my_bfa->get_version();
$stylesheet_url = $my_bfa->get_stylesheet_url();
$prefix = $my_bfa->get_prefix();
$icons = $my_bfa->get_icons();

// Output all available icons.
foreach ( $icons as $icon) {
	echo $icon . '<br />';
}
```

## To Do ##
* Switch from `admin_head_variables` to `wp_localize_script