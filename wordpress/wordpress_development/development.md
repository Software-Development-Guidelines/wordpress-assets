# PLUGIN DEVELOPMENT IN WORDPRESS

(For beginners)

So you decided to become a WordPress plugin developer? What should you know about WordPress before starting? (https://managewp.com/14-surprisi... )

Using WordPress is easy from the user side and because it’s open source it’s also cheap or free. (Some basic plugins are free and some premium or a little more advanced are payable.)

Because it’s cheap a lot of people are using it. Users can use it like a blog platform or a CMS (customer management system) or just as a dynamic page. It’s not really suitable for bigger companies, but because it’s a huge platform it’s quite safe. People are testing the versions for bugs and vulnerabilities and help with the fixes.

Because a lot of people are using WordPress, a lot of developers are developing plugins and themes so you have a huge community behind you for any of your questions. Because it’s so widely used somebody has probably already done what you are trying to do and somebody had the same exact problem that you are facing or a similar problem. So when you get stuck you can get help on the WordPress forums. It takes a little bit of adjusting to program the WordPress way, but overall it’s a platform with the intention to stay and get better and better with every release and you have the option and capability to be a part of it.

## Wordpress installation

This part may seem tricky at first, but actually it is quite easy. You are going to need a local server installation and a WordPress template.

    If you don’t have anything in your mind already – you could try installing EasyPhp server (http://www.easyphp.org/) you have to download EasyPhp Development Server. If everything was configured properly then you could install WordPress through Modules/Recommended Modules section in the server’s home page. If you encounter any problem – Try to check this guide (http://quirm.net/2011/05/05/inst...)
    Updating WordPress is crucial! Because some parts of WP can change and so can change the behavior of your plugin. It is not hard to do, as the process is automatic and all you have to do is to click Auto Update button in the Dashboard menu.
    IDE – You can use PhpStorm as it provides nice WordPress integration if configured properly, but you can also try NetBeans

## How to start

For WordPress to recognize your file as a plugin you need to create a plugins template

You can do it either by using a tool like (WordPress Plugin Boilerplate Generator | Ready to use WordPress Plugin Boilerplate) or by adding the required section manually at the beginning of your php file. You don’t need to end the php file with closing tags (?>), it is recommended that you don’t use closing tags (something to do with the WP parsing).

    */<?php
    /*
     * Plugin name: plugin name
     * Plugin URI: plugin URI
     * Description: Description of what this plugin does
     * Version: Numerical value like 1.0
     * Author: Name of the author
     * Author URI: URI of the author
     * License: GPL2 (it’s usually GPL2 because of the WP open platform)
     */

Your best friends while programing in WordPress are Codex https://codex.wordpress.org/ and WordPress Stack exchange http://wordpress.stackexchange.com/. There is also the official WordPress support forum, but responses there are quite slow, but you can maybe find an answer to your questions (https://wordpress.org/support/).

WP uses hooks which are filters and actions to implement things to the WP pages. (http://codex.wordpress.org/Plugi...).

The two hooks that every plugin needs to have are register_activation_hook(); (https://codex.wordpress.org/Func...) and register_deactivation_hook(); (https://codex.wordpress.org/Func...) .

This is used when a plugin is installed and then is activated or deactivated by user.

The parameters for the function are two, the first one is the file path of a plugin and the second one is the function. So the hook looks like:

    register_activation_hook(__FILE__, name_of_the_function);

And the same is with the deactivation hook. When you use the function in the activation hook it means that something needs to happen immediately when the plugin is activated. Usually it is used for creating custom tables which need to be present right away. Don’t forget to drop custom tables when the user deletes the plugin. To do that you create a new php file called uninstall.php and add the code:

    //only execute the contents of this file if the plugin is really being uninstalled
    if( !defined( 'WP_UNINSTALL_PLUGIN' ) ) {
    exit ();
    }
    //Note!!
    //Activation and deactivation hooks are added automatically if you use wppb.me template tool.

Actions (https://codex.wordpress.org/Plug...) are used when you want to add something to the WP page like external scripts, styles, custom post types, admin menus etc. It has four parameters but just two are usually used, the first one is defined by WP and you need to use Codex to find which one you need, but the most common are init (at the initialization), admin_menu (when admin menu is being processed), wp_ajax_your_name (for the ajax calls), add_meta_boxes (for adding meta boxes), save_post (used for saving the data), the_content (used when you need to add something to the front end).

    add_action( $hook, $function_to_add, $priority, $accepted_args );

(https://codex.wordpress.org/Func...)

Usually it is just the first two parameters:

    add_action('init', function_name);

Filters are also somewhat actions. (https://wordpress.org/support/to..., http://wordpress.stackexchange.c..., http://code.tutsplus.com/article... etc.)

They accept four arguments, but not all are required. Usually the first two are used.

    add_filter($hook, $function_to_add, $priority, $accepted_args);
    add_filter('the_title', 'function_name');

Function names should always have some unique prefixes, so different plugins with the same function names do not collide. For example, if you are developing a plugin and you named the plugin my new plugin, your functions should all be something like mnp_name_of_the_function.

## Adding external files to your plugin

In WP you can't just simply include an external js file like you would do in php with include 'name_of_the_file'; you can include a php file with the include, require, require_once statement, depending on your needs.

For including js files, using ajax and jQuery you need to use a hook for registering or enqueueing a script. For external files you add a hook usually on admin or admin init or just init, so there are different ways of doing it. You can do it this way:

    add_action('admin_init', function_name); 
    function function_name(){
    wp_register_style( 'name_of_the_script'', plugins_url('css/admin-styles.css', __FILE__) );
    wp_register_script('name_of_the_script', plugins_url('js/admin.js', __FILE__));
    }

The name of the script should be unique as it is used as a handle for later use with wp_enqueue_script(). Plugins_url is used for the file path, where the file is located. You don't want to hardcode it, because you never know where the user will have its folders, because WP is flexible with its folders.

wp_enqueue_scripts is the proper hook to use when enqueuing items that are meant to appear on the front end. Despite the name, it is used for enqueuing both scripts and styles.

    add_action('wp_enqueue_scripts', 'function_name');
    function function_name(){
    wp_enqueue_style('core','name_of_the_css.css');
    wp_enqueue_script(‘name-js', 'name_of_the_file.js');
    }

Usually you do two separate add_action with one name for scripts and the other for styles, but just for showing the example I did it in one function.

For using ajax and jQuery in WordPress with your js file, you need to tell wp what you want to use. So in the action and function we created above, we need to add an array in which we add the jQuery. JQuery is already in WP, so we just need to tell WP that our js is using jQuery and we do that by adding the jQuery into an array of our js function enqueue.

    add_action('admin_init', function_name); 
    function function_name(){
    wp_enqueue_script('name_of_the_script', plugins_url( 'js/admin.js', dirname( __FILE__ ) ), array( 'jQuery' ) );

For ajax it’s a little more complicated and I suggest you read carefully about it on the net and don’t rely on my data here, because ajax still confuses the f* out of me. (http://solislab.com/blog/5-tips-...)

In the function above you add after the js enqueuer a new line of code:

wp_localize_script('name_of_the_script', 'Handle_name', array( 'ajaxurl' => admin_url( 'admin-ajax.php' )

admin-ajax.php handles all the ajax for the WordPress, so you need to call it.

The localize script is used when you want to access php data in js files, so to make php data available in your js files. You need to use wp_register or wp_enqueue so that wp_localize could work. In the parameters we need to add the name of the script which is the same as the name of js call. The second parameter is the handle which you will use in ajax like:

    Handle_name.ajaxurl

(https://codex.wordpress.org/AJAX...)

Then you need to handle the ajax. So first you need to hook it with add_action.

wp_ajax_nopriv_{name} is called when registered and unregistered users have access to the ajax. If you want only registered users to have access you then use wp_ajax_{name}.

    add_action( 'wp_ajax_nopriv_name_ajax', name_ajax_handle' );
    function name_ajax _handle(){
    	//handle the ajax request
    }

That is what you need to do in the php file of your plugin. In the js file of your plugin you then need to use the handles you defined in php file for it to work.

So it looks something like:

(snippet of code)

    $.post(
     Handle_name.ajaxurl,
     {
     action: ' name_ajax ',
     award_action: 'accept'
     }

award_action: 'accept' is handled in the function name_ajax _handle. I suggest to read about ajax in the link provided and then google for some good examples that use ajax and then try it. Hope my explanation doesn’t confuse you more.

## Working with Database in WP

Working with database in WP is quite easy. You don’t need to connect with database like you do in php, where you need to specify the database, username, password and open connection and close it. You don’t need to do:

    function openDatabaseConnection(){
    	$localhost = 'ip';
    	$user ='user_name';
    	$pass ='password';
    	$dbname='database_name';
    $conn = mysqli_connect("$localhost","$user", "$pass" , "$dbname");
    	if(mysqli_connect_errno())
    		{
    			echo "Error: " .mysqli_connect_error();
    		} 
    	return $conn;
    }
    function closeDatabaseConnection($conn){
    	mysqli_close($conn);
    }

WordPress has a variable called $wpdb to work with database. So anytime you need to connect to wp database you do:

    global $wpdb;

and then the sql statement like:

    $data = $wpdb->get_results("SELECT * FROM wp_users");

The one thing you need to be careful of is the prefixes. WP uses prefixes for their database, the default prefix is wp, but WordPress allows users to define their own prefixes, so it can be anything. Because you are developing a plugin you don’t know what the database will be, so you need to use a prefix, which looks for a prefix of the database and assigns it to the table name.

    $tablename=$wpdb->prefix."name_of_the_table";

So in your sql statement you then do:

    $data = $wpdb->get_results("SELECT * FROM $tablename");

With $wpdb->get_results you are selecting generic, multiple rows results. The function returns the entire query result as an array. You can get different results with different selecting options. For example if you want to select a row you would use $wpdb->get_row.

It uses the standard sql queries like INSERT, REPLACE, UPDATE and DELETE. It also has a prepare function which protects from sql injection attacks.

If you want to see sql errors or hide them you use $wpdb->show_errors(); or $wpdb->hide_errors();

You can also print errors if they are any generated with the most recent query with $wpdb->print_error();.

For more on WordPress database: Class Reference/wpdb

## Custom post types

When you are developing a plugin you will probably need to use a custom post type for adding a custom post to the WordPress page. Custom post types allow you more flexibility and you can expand the functionalities for your page. (https://codex.wordpress.org/Post...)

Post type can be post, page, attachment, revision and navigation menu (can be any other information stored in a custom post type). Usually, at least at the beginning, you will be working with post, page and navigation menu, maybe attachment.

To create a custom post type you need to follow the regulations in codex.

You first need a hook which will do a custom post type on initialization.

add_action('init', 'function_name'); and in the function you define your custom post type.

Example:

    function function_name() {
     	$labels = array(
    		'name' =>_x('name_of_custom_post', 'post type general name'),
    		'singular_name' =>_x('name_of_custom_post', 'post type singular name'),
    		'add_new' =>_x('name_for_add_new_post'),
    		'add_new_item' =>__('name_for_add_new_item'),
    		'edit_item' =>__('name_for_edit_post'),
    		'new_item' =>__('name_for_new_item'),
    		'view_item' =>__('name_for_view_item'),
    		'search_items' =>__('name_for_search_item'),
    		'not_found' =>__('text_to_display_for_not_found'),
    		'not_found_in_trash' =>__('Nothing found in Trash')
    	);
    	$args = array(
    		'labels' => $labels,
    		'public' => true,
    		'publicly_queryable' => true,
    		'show_ui' => true,
    		'query_var' => true,
    		'menu_icon' => 'dashicons-welcome-learn-more',
    		'rewrite' => true,
    		'capability_type' => 'post'
    		),
    		'hierarchical' => false,
    		'menu_position' => 75,
    		'supports' => array('title','editor','thumbnail','page-attributes'),
    		'has_archive'=>true
    	 ); 
    	//registering the custom post type
    	register_post_type( 'slug_name_for_custom_post_type' , $args );
     	flush_rewrite_rules();

So in the $labels array we just define the label names for our custom post type, what the user will see and in the $args array we define the look of the custom post type and some of its functions. In the above example there are just a few options, for all the options refer to codex. So for example show_ui=>true generates the default user interface for the custom post type and menu_position=>75 means that the menu will be shown in admin menu below tools menu. (https://codex.wordpress.org/Func...) You can also add capabilities to restrict access. If you want to allow access only for admin and editor, or just for admin or just for author etc.

flush_rewrite_rules(); is used for automatic flushing of the WordPress rewrite rules (usually needs to be done manually for new custom post types).

## Custom taxonomy

(https://codex.wordpress.org/Func...)

When you have a CPT (custom post type) you usually have some custom taxonomies to go along. You can use the same hook and function as for the custom post type and just add after flush_rewrite_rules(); a new line of code register_taxonomy($taxonomy, $object_type, $args);

Example:

    register_taxonomy(
     'name_of_taxonomy',
     array('slug_of_CPT'), 
     array(
     'label'=>'name_to_show',
     'labels'=>array(
    		'name'=> _x( 'name' ),
    		'singular_name'=>'name_single',
    		'menu_name'=>__('name_to_show_in_menu')
    		),
    	'show_ui'=>true,
    	'hierarchical'=>true,
    	'rewrite'=>array('slug'=>'name'),
     ));

Let’s look at some options. Again, in the example above there are not all available options. To see or use all available options check the codex (link above). The first parameter is the name of your custom taxonomy, it can be anything, the second parameter is the slug name of CPT, which you previously added to the CPT (slug_name_for_custom_post_type) in the register_ post_type, first parameter. You need to add the slug name of your CPT so that WP knows where custom taxonomies belong. If you want to add custom taxonomy to “ordinary” post, you just put the slug of that post (edit). Then we have array of labels with the name of taxonomy, its singular name and menu name (the name that shows in the admin menu). The important part of this example is the hierarchical=>true options. This means that our custom taxonomy is defined as category, which means that it has descendants. If we set hierarchical=>false it means that custom taxonomy is defined like tags, which means it doesn’t have descendants. You can add as many taxonomies as you wish, but for each taxonomy you need to add new register_taxonomies and its parameters.

You can also add the capabilities to taxonomies if you want to restrict access.

## Roles and capabilities

WP uses different roles for registered users. Super Admin has all privileges and can do anything. Administrator is right behind them with 6 less capabilities. Editor is right behind the administrator and has limited capabilities but still has more than an author, contributor and subscriber. Then it’s the author and contributor and with the least privileges is the subscriber which can only read posts. Depending on the user role and capability the wp admin area changes. Not all menus are shown and with that some options and some settings. (https://codex.wordpress.org/Role...).

If for some reason you want to allow the author to moderate all comments on the page, you can add the capability to it. You need to code it or use a plugin which does that for you. I will not go into detail about roles and capabilities because it depends on what you need them for. Usually you don’t need to create new roles or capabilities, because you as administrator of the page have the “power” to assign roles to registered users. But sometimes you need to create new role or just add some capability so check the link if you need to add a new role: https://codex.wordpress.org/Func.... Maybe good plugin to look at the code for roles and capabilities is Members plugin (https://wordpress.org/plugins/me...).

You need to keep the roles and capabilities in mind when you create things which have the capability parameter like admin menus.

## Admin menus

(https://codex.wordpress.org/Admi...) To create an admin menu you of course need a hook for admin menu.

    add_action('admin_menu','name_of_the_function');

If you are not using a CPT you add a menu page in the function, which is like the first page of the menu.

    add_menu_page( $page_title, $menu_title, $capability, $menu_slug, $function, $icon_url, $position );

And then add submenus if you need them. If you don’t need a submenu you can just have a menu page.

Example function:

    function name_of_the_function(){
    add_menu_page('name_of_page', 'menu_name', 'capability', 'menu_slug', 'name_of_function', 'dashicons-visibility');
    }

We only used 6 parameters, because we don’t need the position. However if we want to position our menu we just write the number for the position we want our menu to be shown after.

2 - Dashboard

4 - Separator

5 - Posts

10 - Media

15 - Links

20 - Pages

25 - Comments

59 - Separator

60 - Appearance

65 - Plugins

70 - Users

75 - Tools

80 - Settings

99 - Separator

For the menu icon we use the dashicons (https://developer.wordpress.org/...). If we want to add submenu to our menu we just add a new line of code after the add menu page. (https://codex.wordpress.org/Func...)

add_submenu_page($parent_slug, $page_title, $menu_title, $capability, $menu_slug, $function );

add_submenu_page('menu_slug ', 'name_of_the_page', 'name_of_the_menu', 'manage-options','submenu_slug','function_name');

Because we are adding a submenu to a menu created with add_menu_page() the first submenu page will be a duplicate of the parent add_menu_page(). If we want a submenu in that way, you should first create a duplicate of add_menu_page() and then add a new submenu page.

function name_of_the_function(){

add_menu_page('My page', 'My page', 'manage-options', 'my-menu-slug', 'dashicons-visibility');

add_submenu_page('my-menu-slug', 'My page', 'My page', 'manage-options', 'my-menu-slug','my_function');

add_submenu_page('my-menu-slug', 'My submenu', 'My submenu', 'manage-options','submenu_slug','my_second_function');

}

If you have a CPT and you want to add a submenu to CPT menu page, then your submenu call should look like:

add_submenu_page('edit.php?post_type=name', 'Page name', 'Page name', 'manage-options','slug','function_name');

The change is the first parameter where you need to write the slug of your CPT and define the page. So edit.php is for the custom post type and then post_type=slug_name of the custom post type that you assigned when creating the CPT.

Capability manage-option is just for super admin and administrator. So if you want to allow access to your admin menu and submenus to other roles/users, you need to change the capability. In the Roles and capabilities link all the capabilities are listed and you choose the capability which the roles have in common. So if you want to allow access to admin menus to administrator and editor your capability will be moderate_comments. Don't pay attention to the name. In this case it doesn't mean anything.

In the functions of your menu and submenu you do the coding. What is happening on that page when user clicks on it.

You can also add an options page which is used when you plugin needs to set some settings to work correctly.

    add_options_page( $page_title, $menu_title, $capability, $menu_slug, $function);

Usually the options page is shown in the Settings admin menu.

    add_options_page('My plugin setting', 'Settings plugin', 'manage_options', 'my-menu-slug', 'my_options_function');

If you want to follow WP way of showing the admin pages you need to use their style.

Example:

    function my_options_function(){
    ?>
    Settings for plugin
    }
    add_action('admin_init', 'settings_init');
    function settings_init(){
    register_setting( 'plugin_options', 'name'); 
    }

If you have one field you need to call the register_setting only once, if you have more fields you need to call it as many times as you have fields. So for each field it is one call. You need to be careful of the first parameter in the register_settings call. The first parameter must match the settings_fields and do_settings_fields and the second parameter is the name of your field. To get the options for options page you just call the get_option('name_of_the_field');

## Add Meta boxes

Whenever you want to add custom data or custom field to wp (in add new post you want to add something) you use meta boxes or custom fields. I haven’t done the custom fields so I will write just about meta boxes.

You again need a hook to just add a meta box add_action('add_meta_boxes','function_add'); and then in your function you add the meta box.

    function function_add(){
    	//slug, name, callback function, post type and priority-where to show the meta box
    	add_meta_box('meta_box_slug', 'Meta box name', 'function_show', 'slug-CPT', 'normal' );
    }

In the function function_show you then actually display the meta box on the admin page.

    function function_show($post){
    //getting the content of a post where metabox is
    $stored_meta = get_post_meta( $post->ID );
    //html code for actually showing the metabox
    ?>
    }

Function show accepts one parameter which is $post. This is so that we can get the post ID. We could also do a function without the parameter and then just call the global $post in the function.

But we are not finished yet. We now have the meta box registered and shown on the page, but if we click the Save post or Update nothing happens with our meta box. That is because we need to save the meta box with the post into database. We do that with another hook add_action('save_post','meta_save'); and then in function meta_save we do the saving. We are checking if its auto save or revision then just return, do nothing, but if it’s not auto save and not revision and the meta box field is set, then we update the post meta. Post meta is in the wp database named wp_postmeta and you can see your newly created meta-text.

    function meta_save($post_id){
    $is_autosave = wp_is_post_autosave( $post_id );
    $is_revision = wp_is_post_revision( $post_id );
    if ( $is_autosave || $is_revision) {
     return;
     }
    if( isset( $_POST[ 'meta-text' ] ) ) {
    update_post_meta( $post_id, 'meta-text', $_POST[ 'meta-text' ] );
     }
    }

But still we are not finished. If you don’t want to display meta box content on the frontend then we are finished and the meta box is created and saved. However if you want to display meta box content on the frontend, on the wp page, you need to add it to the theme. Because you are developing a plugin and you don’t have access to the themes user are using you can’t hardcode it. Here we use action for adding things to the content. So we need to use filter the_content to add something to the content of a page. Action is add_action('the_content', 'meta_content'); and in the function we need to decide where we want to show it. In this example we are showing it in the single.php, single post page.

    function meta_content($content){
    if(is_single()){
    global $post;
    $value=get_post_meta($post->ID, 'meta-text', true);
    if($value){
    $content .= $value;
    }
    }
    return $content;
    }

First we are checking if we are on the single page then we are getting the post and the value of our newly created meta box. We are getting the post ID where our meta box is and the value of the meta box field. The true means that the function will return a single result as string. False would mean that the function will return an array of the custom meta boxes. Then we are appending the new value to the content and returning the new + old content. If we do not append it (if we do just $content=$value) we override the content, thus showing just the new value, which is just the meta box without the post content. With variable $content we get the content of the post. (https://codex.wordpress.org/Func..., https://codex.wordpress.org/Func...).

## WIDGETS

The last thing to mention are widgets. They are used to display something on the frontend. Usually they are used with plugins, but that is not the rule. You can create a plugin which only has a widget. To create a widget you borrow WP class, so you extend their class so that is easy to code. Of course you need to register the widget.

    function name_widget_init(){
    	register_widget(name_widget(){
    	}
    }

Then you extend the WP_Widget class and do the coding. The Widget class has 4 functions you will need to use. The first one is the constructor, where you declare widget options and description of a widget, then we call a parent constructor where we declare a css id, name of the widget and our defined options. The second function is used for displaying the widget and it’s always named function form($instance). This function shows our widget in the appearance->widget admin area of WP. Here we add the text areas, boxes, drop downs and so on. We also save the user input. (if we have a text box where user can input the title of the widget). The Next function is a function for updating the old values with new ones and is always named function update($new_instance, $old_instance). Here we are saving the widget form by getting the values from the user and replacing the old instance with a new one (new value). The last function is for showing the widget on the frontend, so you show it to the user and the name is usually function widget($args, $instance). Here you apply some filters to the title and then show the widget at the beginning of the container ($before_widget), we display the title before the title and then close it with after title. Then we put the code relevant for the widget, what will our widget do and then close the showing of widget with echo $after_widget.

    class WP_Widget_name extends WP_Widget {
    function name_Widget(){
    $widget_options = array( 
    'classname'=>'name_class', //CSS class
    'description' => Description of what widget does');
    //we call parents constructor and add our options
    $this->WP_Widget('name_id','name of widget',$widget_options);	
    }
    function form($instance){
    $defaults=array('title'=>'Title'); //setting the default name to Title
    $instance = wp_parse_args((array) $instance, $defaults); //showing the value
     $title=esc_attr($instance['title']); //saving the users input
    //showing the form
    echo '
    Title
    ';	
    }
    function update($new_instance, $old_instance){
    $instance=$old_instance;
    $instance['title']=strip_tags($new_instance['title']);
    return $instance;
    }
    function widget($args, $instance){
    //extracting vars so they are available as variables
    extract($args);
    $title=apply_filters('widget_title',$instance['title']);
    echo $before_widget;
    //before the beginning of the title we show title and end the title
    echo $before_title. $title. $after_title;			
    //here goes the code for the widget, echoes etc.
    //end of the widget
    echo $after_widget;
    		}
    	}
    }

This is for the “basic” stuff with WordPress. Don’t feel frustrated if you need some time to understand it all, WP is always developing and expanding and adding new stuff and new ways of doing things. So don’t think that the way described in the document is the only way, because it’s not. All coders have their styles and you will see what yours is with WP when you start developing your first plugins. It mainly depends on what tutorials you follow and what is their style of coding. However be careful because WP has some rules on how to do things properly. That is way the codex is there.

I am also very new to WP development and still learning so I probably did and do some mistakes in the code and some parts of code could be done better. However you have to start somewhere, so this is it.

Remember that Google is your best friend when it comes to developing for WP. Don’t be afraid to ask/post questions on the forum and if somebody has already done what you are trying to do, don’t be afraid to use their code or part of the code. It’s WP, so all code in usually GPL2 licensed.

Certainly everything that WP enables is not covered in this document, but hopefully it helps just a little bit.

## Good luck on your journey of becoming a WP plugin developer.