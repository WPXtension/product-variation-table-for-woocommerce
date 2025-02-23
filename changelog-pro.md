### = 1.7.0 [22-02-2025] Sunday =
* Update: Settings framework.
* Feature: Table designing options.
* Update:`$options` array.
* Update: Scripts and added necessary trigger.
* Check: WPCS.

### = 1.6.4 [10-02-2025] Monday =
* Fix: `Search...` text to translate for table search box placeholder.
* Feature: Table popup option for global and individual product settings.
* Update: Settings page.
* Update: Scripts and added necessary trigger.
* Tested: WooCommerce 9.6.

### = 1.6.3 [07-01-2025] Tuesday =
* Update: $option['action'][] array edited and replaced `is_in_stock()` with `get_stock_status()`.

### = 1.6.2 [24-12-2024] Tuesday =
* Fix: Minor conditional fix.
* Update: Codebase based on Plugin Check Plugin(PCP).
* Tested: with WooCommerce 9.5.

### = 1.6.1 [11-12-2024] Wednesday =
* Update: Bulk Ajax cart code as per WooCommerce Ajax Cart.

### = 1.6.0 [29-11-2024] Friday =
* Update: Codebase based on Plugin Check Plugin(PCP).
* Feature: Global variation table condition option.
* Feature: Exclude/Include condition option for Shortcode.
* Dev: `pvtfw_row_cart_btn_is` & `pvtfw_row_cart_btn_oos` now located at compatibility.php. Any snippet you are using with the mentioned filter hooks should use `echo` rather than `return`.
* Dev: Removed `pvt_yith_wishlist_cart_btn_reposition` & `pvt_ti_wishlist_cart_btn_reposition` to reposition the Add to Cart Button with wishlists. To avoid WP Code Standard issues.
* Compatibility: WordPress 6.7.

### = 1.5.4 [05-09-2024] Thursday =
* Update: Codebase based on Plugin Check Plugin(PCP).
* Compatibility: WooCommerce 9.2.

### = 1.5.3 [27-07-2024] Saturday =
* Fix: Thumbnail Popup issue after sorting.
* Feature: Vertical Scrollbar.
* Feature: Variation Table height control.
* Tested: with WordPress 6.6.

### = 1.5.2 [11-07-2024] Thursday =
* Tweak: Option array modification to be compatible with the Free version.
* Tested: WooCommerce 9.1.

### = 1.5.1 [02-07-2024] Tuesday =
* Fix: Script fix for Quantity layout.
* Add: Initial value as 0 if Woo Min/Max plugin is enabled and not added min quantity (Quantity Layout).
* Tested: WooCommerce 9.0.2.

### = 1.5.0 [07-06-2024] Friday =
* Security: Checked plugin code using PCP (Plugin Check Plugin).
* Security: Nonce sanitize applied for Ajax bulk cart button.
* Removed: Dropped Magnific popup library and used built-in PhotoSwipe of WooCommerce to display popup image and popup image gallery.
* Added: Woo Min/Max Quantities plugin support.
* Added: Elementor lightbox support.
* Added: Woodmart PhotoSwipe support.
* Tested: WooCommerce 8.9.

### = 1.4.16 [27-04-2024] Saturday =
* Fix: Bulk cart script when a variation is already in the cart and crosses the limit of stock.
* Compatibility: WooCommerce 8.8 & WordPress 6.5.

### = 1.4.15 [27-02-2024] Tuesday =
* Fix: Checkbox JS issue when multiple tables are on the same single product page.
* Compatibility: WooCommerce 8.6.

### = 1.4.14 [21-12-2023] Thursday =
* Update: Bulk Cart script.
* Update: Multisite dependency condition for PVT Free.
* Fix: Bulk Cart button function processing icon.

### = 1.4.13 [13-12-2023] Wednesday =
* Fix: Plugin activation in multisite dependency.
* Compatibility: WooCommerce 8.4.

### = 1.4.12 [05-12-2023] Tuesday =
* Fix: Bulk cart button firing a blank notification.
* Fix: Bulk cart checkbox layout with pre-selected all checkbox script.
* Add: `pvt_yith_wishlist_cart_btn_reposition` & `pvt_ti_wishlist_cart_btn_reposition` to reposition the Add to Cart Button with wishlists.
* Update: Bulk cart hook. Now it is hooked with `pvtfw_after_table_block` & `pvtfw_before_table_block`.
* Update: $options array according to basic `$options` values.

### = 1.4.11 [22-11-2023] Wednesday =
* Feature: Remove disabled quantity field if variation is out of stock.
* Fix: Bulk cart (total price) decimal digit limit.
* Compatibility: WooCommerce 8.3.1.

### = 1.4.10 [14-11-2023] Monday =
* Feature: Add to cart button appearance option to change the add to cart button to only icon, only text, and both icon with text.
* Compatibility: YITH Wishlist, TI Wishlist, and Back In Stock Notifier plugin.
* Update: options array.
* Compatibility: WordPress 6.4 & WooCommerce 8.2.2.

### = 1.4.9 [04-11-2023] Saturday =
* Fix: Bulk button items count and quantity fix for the option both position.
* Update: +/- quantity field compatibility with Astra theme.
* Update: HPOS support.
* Tweak: Initialize quantity field markup.

### = 1.4.8 [24-10-2023] Tuesday =
* Tweak: Updated JS for new +/- quantity field.
* Dev: Updated `$options` (in `content-frontend.php`) as per the standard/lite version `$options` array.
* Compatibility: WooCommerce 8.2+.

### = 1.4.7 [08-10-2023] =
* Feature: Additional columns.
* Compatibility: WordPress 6.3+ & WooCommerce 8.1+.

### = 1.4.6 [02-6-2023] =
* Feature: Display the number of selected items and the total price with the Bulk Cart Button.
* Compatibility: WordPress 6.2+ & WooCommerce 7.7+.

### = 1.4.5 [24-3-2023] =
* Tweak: Typo fix in plugin settings page.
* Feature: Pagination feature added.
* Feature: Layout swticher for table appearance. Now, both checkbox and quantity base bulk cart layouts are available.
* Tested: Compatibility with WooCommerce 7.5
* Added: .pot file inside languages folder.

### = 1.4.4 [22-1-2023] =
* Feature: Exclude out of stock variation from table
* Tested: Compatibility with WooCommerce 7.3

### = 1.4.3 [25-10-2022] =
* Feature: Pre-selected varation feature added
* Tested: Compatibility with WooCommerce 7.0

### = 1.4.2 [10-09-2022] =
* Fix: Typo for inlcude to include. 
* Fix: Error if PVT free/basic is not installed. 
* Feature: Disable PVT for mobile screen.
* Feature: Bulk cart instruction
* Feature: Bulk cart instruction text to set own message or instruction
* Dev: Bulk Cart disabled till checkboxes clicked
* Dev: Added filter for Bulk Cart button text pvtfw_table_bulk_cart_text

### = 1.4.1 [29-07-2022] =
* Dev: Menu location changed and its now under WPXtension Menu
* Tested: WordPress 6.0.1
* Tested: WooCommerce 6.7.0

### = 1.4.0 [28-02-2022] =
* Initial Release
