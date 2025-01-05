> [!IMPORTANT]
> - To run the following code you have to install the PVT - Product Variation Table for WooCommerce at least 1.6.0.
> - It helps when you enable the "Keep Variation Table with Dropdown" option for the "Display Rule" feature.
> - It will display the "Available Options" button after the "Add to Cart" button and "Variation Dropdowns" of the single product page.

### Snippet to display the Available Options button under the Add to Cart Button
```
add_action( 'template_redirect', function(){
	if( !class_exists( 'PVTFW_TABLE' ) && !class_exists( 'PVTFW_AVAILABE_BTN' ) && !class_exists( 'PVTFW_COMMON' ) ){
		return;
	}
	remove_filter( 'disable_pvt_to_show_available_option', '__return_true' );
	$pvtfw_available_btn = PVTFW_AVAILABE_BTN::instance();
	$showAvailableOptionBtn = PVTFW_COMMON::pvtfw_get_options()->showAvailableOptionBtn;
	if ($showAvailableOptionBtn == 'on') {
		remove_action('woocommerce_single_product_summary', array($pvtfw_available_btn, 'available_options_btn'), 11);
		add_action('woocommerce_single_product_summary', array($pvtfw_available_btn, 'available_options_btn'), 31);
	}
}, 99);
```
