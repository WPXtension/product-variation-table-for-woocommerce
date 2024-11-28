# PVT Free version Snippets for YITH Request a Quote Free Plugin

- [For In-Stock Variation Without Add to Cart Button](#for-in-stock-variation-without-add-to-cart-button) - Without Add to Cart Button
- [For In-Stock Variation With Add to Cart Button](#for-in-stock-variation-with-add-to-cart-button) - With Add to Cart Button

## For In-Stock Variation Without Add to Cart Button
```
// PVT filtering In Stock Button
add_filter('pvtfw_row_cart_btn_is', function($default,$product_id, $cart_url, $product_url, $variant_id, $text){

	ob_start();

	if( YITH_Request_Quote()->exists($product_id, $variant_id) ){

		?>
		<div class="yith_ywraq_add_item_response-<?php echo absint($product_id); ?> yith_ywraq_add_item_response_message">
			<?php echo $message = ywraq_get_label( 'already_in_quote' ); ?>
		</div>
		<div class="yith_ywraq_add_item_browse-list-<?php echo absint($product_id); ?> yith_ywraq_add_item_browse_message">
			<a href="<?php echo YITH_Request_Quote()->get_raq_page_url(); ?>"><?php echo ywraq_get_browse_list_message(); ?></a>
		</div>
		<?php
	}
	else{

	?>
		<form class="pvt-yith-quote-support cart">
			<input type="number" id="quantity" class="input-text qty text" name="quantity" value="1" title="Qty" size="4" min="1" max="" step="1" placeholder="" inputmode="numeric" autocomplete="off">
			<input type="hidden" name="add-to-cart" value="<?php echo absint($product_id); ?>">
			<input type="hidden" name="product_id" value="<?php echo absint($product_id); ?>">
			<input type="hidden" name="variation_id" class="variation_id" value="<?php echo absint($variant_id); ?>">


			<div class="pvt-yith-ywraq-add-to-quote add-to-quote-<?php echo absint($variant_id); ?> near-add-to-cart">
				<div class="pvt-yith-ywraq-add-button show" style="display:block">

					<a href="#" class="pvt-add-request-quote-button button" data-product_id="<?php echo absint( $product_id ); ?>">Add to quote</a>
					<img src="/wp-content/plugins/yith-woocommerce-request-a-quote/assets/images/wpspin_light.gif" class="ajax-loading" alt="loading" width="16" height="16" style="visibility:hidden">
				</div>
			</div>

		</form>
		<?php

	}
	echo ob_get_clean();	
}, 10, 6);


// Rewrite version of JS for YITH QUOTE PLUGIN Support
add_action( 'wp_footer', function(){

	?>
		<script id="pvt-yith-quote-support-js" type="text/javascript">

			(function($){

				$(document).on('click', '.pvt-add-request-quote-button', function (e) {
					e.preventDefault();

					var $t = $(this),
						$t_wrap = $t.parents('.pvt-yith-ywraq-add-to-quote'),
						add_to_cart_info = 'ac',
						$add_to_cart_el = null,
						$product_id_el = null;

					if ($t.parents('ul.products').length > 0) {
						    $add_to_cart_el = $t.parents('li.product').find('input[name="add-to-cart"]');
							$product_id_el = $t.parents('li.product').find('input[name="product_id"]');
					} else {
						    $add_to_cart_el = $t.parents('.product').find('input[name="add-to-cart"]');
							$product_id_el = $t.parents('.product').find('input[name="product_id"]');
					}

					if ($add_to_cart_el.length > 0 && $product_id_el.length > 0) { //variable product
						add_to_cart_info = $t.closest('.cart').serialize();
					} else if ($t.closest('.cart').length > 0) { //single product and form exists with cart class
						add_to_cart_info = $t.closest('.cart').serialize();
					}

					add_to_cart_info += '&action=yith_ywraq_action&ywraq_action=add_item&product_id=' + $t.attr('data-product_id') + '&_wpnonce=' + ywraq_frontend.yith_ywraq_action_nonce;
					if (add_to_cart_info.indexOf('add-to-cart') > 0) {
						add_to_cart_info = add_to_cart_info.replace('add-to-cart', 'yith-add-to-cart');
					}

					$.ajax({
						type: 'POST',
						url: ywraq_frontend.ajaxurl,
						dataType: 'json',
						data: add_to_cart_info,
						beforeSend: function () {
							$t.siblings('.ajax-loading').css('visibility', 'visible');
						},
						complete: function () {
							$t.siblings('.ajax-loading').css('visibility', 'hidden');
						},

						success: function (response) { console.log(response);
							if (response.result == 'true' || response.result == 'exists') {

								if (ywraq_frontend.go_to_the_list === 'yes') {
									window.location.href = response.rqa_url;
								} else {
									$t.parent().hide().removeClass('show').addClass('addedd');
									var prod_id = (typeof $product_id_el.val() == 'undefined') ? '' : '-' + $product_id_el.val();
									$t_wrap.append('<div class="yith_ywraq_add_item_response' + prod_id + ' yith_ywraq_add_item_response_message">' + response.message + '</div>');
									$t_wrap.append('<div class="yith_ywraq_add_item_browse-list' + prod_id + ' yith_ywraq_add_item_browse_message"><a href="' + response.rqa_url + '">' + response.label_browse + '</a></div>');
								}

							} else if (response.result == 'false') {
								$t_wrap.append('<div class="yith_ywraq_add_item_response-' + $product_id_el.val() + '">' + response.message + '</div>');
							}
						}
					});


				});


			}(jQuery))

		</script>
	<?php

}, 99 );

// Remove the default YITH Quote button when variable product

add_action('template_redirect', function(){

	if( is_woocommerce() ){
		$adding_to_quote = wc_get_product( get_the_ID() );
		if( is_a( $adding_to_quote, 'WC_Product_Variable' ) ){
			if( $adding_to_quote->is_type( 'variable' ) ){
				remove_action( 'woocommerce_single_product_summary', array( YITH_YWRAQ_Frontend::get_instance(), 'add_button_single_page' ), 35 );
			}
		}
		
	}

});

// Remove the default cart button with each row
add_filter('pvtfw_disable_add_to_cart_button', '__return_true');
```
## For In-Stock Variation With Add to Cart Button
```
// PVT filtering In Stock Button
add_filter('pvtfw_row_cart_btn_is', function($default,$product_id, $cart_url, $product_url, $variant_id, $text){

	ob_start();

	if( YITH_Request_Quote()->exists($product_id, $variant_id) ){

		?>
		<div class="yith_ywraq_add_item_response-<?php echo absint($product_id); ?> yith_ywraq_add_item_response_message">
			<?php echo $message = ywraq_get_label( 'already_in_quote' ); ?>
		</div>
		<div class="yith_ywraq_add_item_browse-list-<?php echo absint($product_id); ?> yith_ywraq_add_item_browse_message">
			<a href="<?php echo YITH_Request_Quote()->get_raq_page_url(); ?>"><?php echo ywraq_get_browse_list_message(); ?></a>
		</div>
		<?php
	}
	else{

	?>
		<form class="pvt-yith-quote-support cart">
			<input type="number" id="quantity" class="input-text qty text" name="quantity" value="1" title="Qty" size="4" min="1" max="" step="1" placeholder="" inputmode="numeric" autocomplete="off">
			<input type="hidden" name="add-to-cart" value="<?php echo absint($product_id); ?>">
			<input type="hidden" name="product_id" value="<?php echo absint($product_id); ?>">
			<input type="hidden" name="variation_id" class="variation_id" value="<?php echo absint($variant_id); ?>">


			<div class="pvt-yith-ywraq-add-to-quote add-to-quote-<?php echo absint($variant_id); ?> near-add-to-cart">
				<div class="pvt-yith-ywraq-add-button show" style="display:block">

					<a href="#" class="pvt-add-request-quote-button button" data-product_id="<?php echo absint( $product_id ); ?>">Add to quote</a>
					<img src="/wp-content/plugins/yith-woocommerce-request-a-quote/assets/images/wpspin_light.gif" class="ajax-loading" alt="loading" width="16" height="16" style="visibility:hidden">
				</div>
			</div>

		</form>
		<?php

	}
	echo ob_get_clean();	
}, 10, 6);


// Rewrite version of JS for YITH QUOTE PLUGIN Support
add_action( 'wp_footer', function(){

	?>
		<script id="pvt-yith-quote-support-js" type="text/javascript">

			(function($){

				$(document).on('click', '.pvt-add-request-quote-button', function (e) {
					e.preventDefault();

					var $t = $(this),
						$t_wrap = $t.parents('.pvt-yith-ywraq-add-to-quote'),
						add_to_cart_info = 'ac',
						$add_to_cart_el = null,
						$product_id_el = null;

					if ($t.parents('ul.products').length > 0) {
						    $add_to_cart_el = $t.parents('li.product').find('input[name="add-to-cart"]');
							$product_id_el = $t.parents('li.product').find('input[name="product_id"]');
					} else {
						    $add_to_cart_el = $t.parents('.product').find('input[name="add-to-cart"]');
							$product_id_el = $t.parents('.product').find('input[name="product_id"]');
					}

					if ($add_to_cart_el.length > 0 && $product_id_el.length > 0) { //variable product
						add_to_cart_info = $t.closest('.cart').serialize();
					} else if ($t.closest('.cart').length > 0) { //single product and form exists with cart class
						add_to_cart_info = $t.closest('.cart').serialize();
					}

					add_to_cart_info += '&action=yith_ywraq_action&ywraq_action=add_item&product_id=' + $t.attr('data-product_id') + '&_wpnonce=' + ywraq_frontend.yith_ywraq_action_nonce;
					if (add_to_cart_info.indexOf('add-to-cart') > 0) {
						add_to_cart_info = add_to_cart_info.replace('add-to-cart', 'yith-add-to-cart');
					}

					$.ajax({
						type: 'POST',
						url: ywraq_frontend.ajaxurl,
						dataType: 'json',
						data: add_to_cart_info,
						beforeSend: function () {
							$t.siblings('.ajax-loading').css('visibility', 'visible');
						},
						complete: function () {
							$t.siblings('.ajax-loading').css('visibility', 'hidden');
						},

						success: function (response) { console.log(response);
							if (response.result == 'true' || response.result == 'exists') {

								if (ywraq_frontend.go_to_the_list === 'yes') {
									window.location.href = response.rqa_url;
								} else {
									$t.parent().hide().removeClass('show').addClass('addedd');
									var prod_id = (typeof $product_id_el.val() == 'undefined') ? '' : '-' + $product_id_el.val();
									$t_wrap.append('<div class="yith_ywraq_add_item_response' + prod_id + ' yith_ywraq_add_item_response_message">' + response.message + '</div>');
									$t_wrap.append('<div class="yith_ywraq_add_item_browse-list' + prod_id + ' yith_ywraq_add_item_browse_message"><a href="' + response.rqa_url + '">' + response.label_browse + '</a></div>');
								}

							} else if (response.result == 'false') {
								$t_wrap.append('<div class="yith_ywraq_add_item_response-' + $product_id_el.val() + '">' + response.message + '</div>');
							}
						}
					});


				});


			}(jQuery))

		</script>
	<?php

}, 99 );

// Remove the default YITH Quote button when variable product

add_action('template_redirect', function(){

	if( is_woocommerce() ){
		$adding_to_quote = wc_get_product( get_the_ID() );
		if( is_a( $adding_to_quote, 'WC_Product_Variable' ) ){
			if( $adding_to_quote->is_type( 'variable' ) ){
				remove_action( 'woocommerce_single_product_summary', array( YITH_YWRAQ_Frontend::get_instance(), 'add_button_single_page' ), 35 );
			}
		}
		
	}

});
```
