# What is a widget?

## WordPress Widget Boilerplate

A few months ago, I contributed an article to WPTuts on [Writing Maintainable WordPress Widgets](http://wp.tutsplus.com/tutorials/widgets/writing-maintainable-wordpress-widgets-part-1-of-2/). The motivation for this series was driven largely by the fact that as much as I love the WordPress community, there are more than a few poorly constructed plugins.

In some cases, this is fine. If you’re planning to quickly throw something together with no plans to continue development after its initial release, you can probably get a way with throwing something functional together.

But if you’re looking to build a plugin that you’ll be maintaining over time, then I believe applying good software development practices is a must. That is, I think that developers should follow [the platform’s API](http://codex.wordpress.org/Widgets_API), use design patterns where applicable, and clearly organize their files.

Over the past year, I’ve written a number of WordPress plugins both for contract and for hobby. During that time, I began creating a boilerplate of code off of which I build most of my widgets.

![wpwb.png](/assets/wpwb.png)

The WordPress Widget Boilerplate features the following:

* File Organization. The Boilerplate ships with both JavaScript sources and stylesheets for both the administrator and the client-side views. It provides a basic localization file to make it easy to localize the plugin. It also includes a stubbed out README that follows WordPress conventions.
* Documented Code. Each file of the plugin and each method of the core code is clearly documented for its purpose in the overall plugin. Additionally, the core code includes various TODO’s to make it easy for your IDE to locate everything you need to populate when working on your plugin.
* API Implementation. The Boilerplate is based on the WordPress API in order to enforce best practices when building on top of the WordPress platform. This makes it easy to develop conventional, familiar code and makes it easy to compare your work with the recommendations of the Codex.

The WordPress Widget Boilerplate is available on [GitHub](https://github.com/tommcfarlin/WordPress-Widget-Boilerplate) and is under active development. Feel free to watch, fork, or contribute. I’m always looking to make it as solid as possible when it comes to plugin development.
