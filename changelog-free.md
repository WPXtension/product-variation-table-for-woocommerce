### = 1.6.0 [13-11-2024] Wednesday =
* Update: Codebase based on Plugin Check Plugin(PCP).
* Dev: `pvtfw_row_cart_btn_is` & `pvtfw_row_cart_btn_oos` now located at compatibility.php. Any snippet you are using with the mentioned filter hooks should use `echo` rather than `return`.
* Compatibility: WordPress 6.7.

### = 1.5.4 [10-09-2024] Tuesday =
* Fix: PHP Fatal error:  Uncaught Error: Class "PVTFW_COMMON" not found.

### = 1.5.3 [05-09-2024] Thursday =
* Update: Codebase based on Plugin Check Plugin(PCP).
* Compatibility: WooCommerce 9.2.

### = 1.5.2 [27-07-2024] Saturday =
* Added: `pvtfw_admin_after_horizontal_scrollbar` hook for settings.
* Compatibility: WordPress 6.6.

### = 1.5.1 [11-07-2024] Thursday =
* Add: Display Stock Status if the Manage Quantity and Stock Quantity fields are not enabled and added.
* Update: Script and readme.txt .
* Compatibility: WooCommerce 9.1.0.

### = 1.5.0 [07-06-2024] Friday =
* Security: Checked plugin code using PCP (Plugin Check Plugin).
* Security: Nonce sanitize applied for Ajax Add-to-cart button.
* Codebase: Changed the content-tbody.php to display quantity input field.
* Dev: Added `pvt_global_attribute_terms`, `pvt_custom_attribute_terms` filters. Replaced this hook `pvt_print_plus_minus_qty_field` with `pvt_print_qty_field`
* Fix: Subtotal column calculation issue if different quantity number added for specific variation.
* Fix: Subtotal JS for Stock out variation.
* Compatibility: WooCommerce 8.9

### = 1.4.22 [27-04-2024] Saturday =
* Fix: Notice after carting an item to display the variation not only the product name.
* Compatibility: WooCommerce 8.8 & WordPress 6.5.

### = 1.4.21 [27-02-2024] Tuesday =
* Fix: Decoded term slug if another language applied.
* Fix: Hidden quantity field if stock amount is 1.
* Compatibility: WooCommerce 8.6.

### = 1.4.20 [31-12-2023] Sunday =
* Update: Multisite condition revised. Now it will work if enabled in Network Admin or enabled in sub sites (but disabled in Netword Admin).
* Update: Shortcode will work on any product type (Simple/Grouped/External). But the given `Product ID` should be a variable product.
* Tweak: Updated the icon font-family to resolve the conflict with another theme/plugin (Which is using the same unicode).

### = 1.4.19 [13-12-2023] Wednesday =
* Fix: Plugin activation in multisite dependency.
* Compatibility: WooCommerce 8.4.

### = 1.4.18 [05-12-2023] Tuesday =
* Fix: +/- quantity field script file.
* Fix: +/- quantity field style file.
* Added: Hook `pvtfw_available_options_btn_text` for translating the dynamic text.
* Added: Hook `pvtfw_before_table_block` & `pvtfw_after_table_block`.
* Added: Whole table (up & down part) wrapped with `pvtfw_init_variation_table` `<div>` for better control.

### = 1.4.17 [22-11-2023] Wednesday =
* Update: content-tbody.php script, to pass more value to `pvt_woocommerce_quantity_input_args`.
* Fix: Subtotal column price for undefined value when quantity field is disabled.
* Compatibility: WooCommerce 8.3.1.

### = 1.4.16 [14-11-2023] Tuesday =
* Update: content-tbody.php script.
* Update: Translation correction.
* Fix: Design issue of +/- button of quantity field for the block themes like- Twenty Twenty-Four, Twenty Twenty-Three, etc.
* Compatibility: WordPress 6.4 & WooCommerce 8.2.2.

### = 1.4.15 [04-11-2023] Saturday =
* Update: +/- quantity field compatibility with Astra theme.
* Update: HPOS support.
* Tweak: Initialize quantity field markup.

### = 1.4.14 [24-10-2023] Tuesday =
* Feature: Updated the +/- quantity field option. PVT has its own quantity field now.
* Feature: Enabel or, disable Available Options title at the top of the table.
* Tweak: Updated the JS for new field.
* Tweak: Updated content-tbody.php, argumets added for `wp_parse_args`.
* Tweak: Added filter for basic input value `woocommerce_quantity_input_min`.
* Dev: Added filter `pvt_print_plus_minus_qty_field`.
* Compatibility: WooCommerce 8.2+.

### = 1.4.13 [08-10-2023] Sunday =
* Update: `content-tbody.php` file optimized.
* Added: Reset link for column ordering & plugin settings.
* Fix: Minor CSS for Plugin Settings Page.
* Dev: `woocommerce_quantity_input_args` changed to `pvt_woocommerce_quantity_input_args`.
* Tweak: Conditional variable replacement for code optimizing.
* Compatibility: WordPress 6.3 & WooCommerce 8.1+.

### = 1.4.12 [12-07-2023] Sunday =
* Fix: `Uncaught Error: Call to undefined method` for product types except variable proudct type.

### = 1.4.11 [11-07-2023] Saturday =
* Fix: `Warning Invalid argument supplied for foreach()` if prices are empty. Now PVT will act as default WooCommerce does.
* Dev: Changed files- `class_pvtfw_print_table.php` & `class_pvtfw_available_btn`
* Fix: Removed condition of checking only for price column heading is pressed or not for sorting.

### = 1.4.10 [14-06-2023] Wednesday =
* Fix: Sorting price column, in past it was getting prices as string.
* Added: Sorting arrow for the subtotal column.
* Compatibility: WooCommerce 7.8+.

### = 1.4.9 [02-06-2023] =
* Enhancement: Useful Plugins submenu kept at the bottom of the list for easy access.
* Compatibility: WordPress 6.2+ & WooCommerce 7.7+.

### = 1.4.8 [24-03-2023] =
* Tweak: Typo fix in plugin settings page.
* Dev: Script updated.
* Enhancement: Plugin version badge.
* Enhancement: Shortcode listed in plugin setting page.
* Added: .pot file inside languages folder.
* Compatibility: WooCommerce 7.5.

### = 1.4.7 [22-01-2023] =
* Dev: A filter added `pvt_skip_some_variation`
* Enhancement: Plugin settings and badge.
* Compatibility: WooCommerce 7.3.

### = 1.4.6 [08-12-2022] =
* Fix: Warning: Undefined array key
* Compatibility: WooCommerce 7.1.1.

### = 1.4.5 [26-10-2022] =
* Enhancement: Displaying included tax with price in sub total column. If it enabled from WooCommerce settings.
* Dev: `[pvtfw_table_display]` shortcode updated, can be passed product id to get a specific products variations. For example: `[pvtfw_table_display id="91"]`.
* Dev: Added `pvtfw_num_of_decimal` filter hook, to remove decimal number after the price.
* Dev: Added more filter hooks for option array: `pvtfw_table_sku`, `pvtfw_table_variation_description`, `pvtfw_table_attributes`, `pvtfw_table_dimensions_html`, `pvtfw_table_weight_html`, `pvtfw_table_availability_html`. `$single_variation` parameter also passed for the listed hooks to get individual variation data.
* Dev: Added classes for <tr> of variations table for better styling. For example: `<tr class="pvt-tr-{product_id}-{unique_id}">`

### = 1.4.4 [10-09-2022] =
* Fix: Subtotal issue if currencey is not US Dollar or tax include/exclude text added with the pice.
* Fix: Plugin Settings Page issue with boost.io plugin
* Dev: Added `pvtfw_subtotal_with_currency_symbol` filter hook, to change price and currency symbol position.

### = 1.4.3 [25-08-2022] =
* Fix: Error message `is_type() on null` in Elementor Tempalte preview mode.
* Fix: Image CSS on Advanced Tab in Plugin Settings page.
* Dev: Added `pvtfw_action_th_title` filter hook, to add a column title in action column.
* Compatibility: WooCommerce 6.8.0.

### = 1.4.2 [29-07-2022] =
* Feature: Redirect to the cart page after successful addition [if enabled from woocommerce settings].
* Compatibility: WooCommerce 6.7.0.
* Dev: Changed the plugin page title and inserted inside the `wrap`.
* Dev: Plugin setting page moved from WooCommerce menu to WPXtension menu.
* Dev: Sidebar added in plugin setting page.
* Dev: Added filters [`woocommerce_quantity_input_min`, `woocommerce_quantity_input_max`] to change the minimum and maximum value of basic and +/- quantity field.
* Fixed: Typing mistake `Our of Stock` in Out of Stock Button string. 
* Fixed: Translate facility added for some hard string. 

### = 1.4.1 [09-06-2022] =
* Compatibility: whols plugin by WooLentor compatibility
* Compatibility: WooCommerce 6.5.1
* Dev: Applied `get_price_html` rather than PVT price html
* Dev: Kept backward compatibility for previous PVT price html

### = 1.4.0 [12-03-2022] =
* Compatibility: added [hook]- `pvtfw_price_html` for Woo Discount Rules plugin
* Dev: Passed value for `pvtfw_row_cart_btn_oos` and `pvtfw_row_cart_btn_is` to write own button style or add more button
* Dev: Code restructured
* Compatibility: WooCommerce 6.3.1

### = 1.3.15 [21-02-2022] =
* Fixed: Add to cart & Out of stock dynamic text translation issue.
* Fixed: Applied filter for column title SKU and made it translatable.
* Compatibility: Tested with WordPress 5.9
* Compatibility: Tested with WooCommerce 6.2.0

### = 1.3.14 [11-12-2021] =
* Fixed: Variation display issue (if not enabled)

### = 1.3.13 [26-10-2021] =
* Feature: Subtotal Column added to display instant price after updating quantity field
* Fixed: Infinity cart spinner after unchecking Cart Confirmation Notice for Ashe theme
* Fixed: Removed single_add_to_cart_button class, to avoid unwanted behaviour of cart button
* Fixed: Blank Mini cart issue resolved
* Fixed: Table width applied with shortcode also
* Dev- Backward compatibility added and heredoc replaced with sprintf
* Dev- Filter hook added for button string

### = 1.3.12 [16-09-2021] =
* Fixed: Infinity cart spinner after unchecking Cart Confirmation Notice
* Fixed: Divi Theme mini cart quantity update

### = 1.3.11 [14-09-2021] =
* Feature: Added scrollbar option
* Feature: Added table minimum width option
* Feature: Added an option to stop breakdown of table row on mobile screen

### = 1.3.9 [27-07-2021] =
* Filter hooks added to change Table title (for both desktop & mobile)
* [hooks]- pvtfw_columns_labels | pvtfw_image_link_title | pvtfw_price_html_title | pvtfw_variation_description_title | pvtfw_dimensions_html_title | pvtfw_weight_html_title | pvtfw_availability_html_title | pvtfw_quantity_title | pvtfw_action_title

### = 1.3.8 [12-07-2021] =
* Fixed: Shortcode to display it on exact place

### = 1.3.7 [11-07-2021] =
* Shortcode: [pvtfw_table_display] added for displaying table 

### = 1.3.6 [26-06-2021] =
* Priority pushed for some action hook
* Rewrite facility added for hooks
* Replaced `init` with `template_redirect` to push condition

### = 1.3.5 [15-06-2021] =
* Column Position Switching Option Added
* Added two new hooks to display before/after Variation Table
* Translation problem for mobile version fixed
* Display notice on frontend issue solved (if woocommerce not activated & installed)

### = 1.3.4 [25-05-2021] =
* OOP implemented.
* Table header on/off added
* Cart Notice on/off added

### = 1.3.3 =
* Disbale Button if variant out of stock.
* Two more table positioning added.

### = 1.3.2.1 =
* Fixed attribute so it now uses the actual name/label and not the slug.

### = 1.3.2 =
* Plus Minus button added for quantity.
* Validation added for quantity using max stock quantity.
* Translate issue solved

### = 1.3.1 =
* Languages folder added
* Added textdomain with hard coded string
* Some minor logical changes with condition

### = 1.3.0 =
* Add to cart AJAX notice problem solved
* Added notification for both Success and Error

### = 1.2.8 =
* Compatibility issues fixed with currency changer plugin

### = 1.2.7 =
* Mobile variation attibute title issue solved
* Table view updated with some css fixing
* Cart button padding issue minimized

### = 1.2.5 =
* WooCommerce dependency issue minimized

### = 1.2.2 =
* CSS for loader

### = 1.2.1 =
* JS for loader

### = 1.2.0 =
* Attribute label displayed rather than slug.
* Minor css changes.
* Ajax cart animated loader implemented.

### = 1.1.7 =
* Minor css issues solved.
* Ajax cart facility implemented.

### = 1.1 =
* Compatible with PHP versions.
* Fix minor syntax error issues in older version of php.

### = 1.0 =
* Initial version.
