> [!IMPORTANT]
> To run the following code you have to install the PVT - Product Variation Table for WooCommerce at least 1.6.0.


### Snippets for YITH Request a Quote Free Plugin

- [For In-Stock Variation Without Add to Cart Button](#for-in-stock-variation-without-add-to-cart-button-free) - Without Add to Cart Button
- [For In-Stock Variation With Add to Cart Button](#for-in-stock-variation-with-add-to-cart-button-free) - With Add to Cart Button
- [For Out-of-Stock Variation Without Out-of-Stock Button](#for-out-of-stock-variation-without-out-of-stock-button-free) - Without Out Of Stock Button
- [For Out-of-Stock Variation With Out-of-Stock Button](#for-out-of-stock-variation-with-out-of-stock-button-free) - With Out Of Stock Button

### Snippets for YITH Request a Quote Premium Plugin

- [For In-Stock Variation Without Add to Cart Button](#for-in-stock-variation-without-add-to-cart-button-premium) - Without Add to Cart Button
- [For In-Stock Variation With Add to Cart Button](#for-in-stock-variation-with-add-to-cart-button-premium) - With Add to Cart Button
- [For Out-of-Stock Variation Without Out-of-Stock Button](#for-out-of-stock-variation-without-out-of-stock-button-premium) - Without Out Of Stock Button
- [For Out-of-Stock Variation With Out-of-Stock Button](#for-out-of-stock-variation-with-out-of-stock-button-premium) - With Out Of Stock Button
- [Role base quote button](#role-base-quote-button)

### For In-Stock Variation Without Add to Cart Button (Free)
[^ Go to Top](#snippets-for-yith-request-a-quote-free-plugin)
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
### For In-Stock Variation With Add to Cart Button (Free)
[^ Go to Top](#snippets-for-yith-request-a-quote-free-plugin)
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

// Remove the default YITH Quote button, when variable product

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
### For Out-of-Stock Variation Without Out-of-Stock Button (Free)
[^ Go to Top](#snippets-for-yith-request-a-quote-free-plugin)
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

// Remove the default out-of-stock button with each row
add_filter('pvtfw_disable_out_of_stock_button', '__return_true');
```
### For Out-of-Stock Variation With Out-of-Stock Button (Free)
[^ Go to Top](#snippets-for-yith-request-a-quote-free-plugin)
```
// PVT filtering In Stock Button
add_filter('pvtfw_row_cart_btn_oos', function($default,$product_id, $cart_url, $product_url, $variant_id, $text){

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

// Remove the default YITH Quote button, when variable product

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
### For In-Stock Variation Without Add to Cart Button (Premium)
[^ Go to Top](#snippets-for-yith-request-a-quote-premium-plugin)
```
add_filter('pvtfw_row_cart_btn_is', function($default,$product_id, $cart_url, $product_url, $variant_id, $text){
	
	global $product;

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
		$exists = $product->is_type( 'variable' ) ? false : YITH_Request_Quote()->exists( $product_id, $variant_id );
		$rqa_url = YITH_Request_Quote()->get_raq_page_url();
		$label_browse = ywraq_get_label( 'browse_list' );
		

	?>
		<form class="pvt-yith-quote-support cart">
			
			<?php
				$variations = new WC_Product_Variation($variant_id);
				foreach( $variations->get_variation_attributes() as $key => $variation ){
					echo "<input type='hidden' name='".$key."' value='".$variation."'>";
				}
			?>
			
			<input type='hidden' name='quantity' class='qty' value='1'>
			<input type="hidden" name="add-to-cart" value="<?php echo absint($product_id); ?>">
			<input type="hidden" name="product_id" value="<?php echo absint($product_id); ?>">
			<input type="hidden" name="variation_id" class="variation_id" value="<?php echo absint($variant_id); ?>">


			<div class="pvt-yith-ywraq-add-to-quote add-to-quote-<?php echo absint($variant_id); ?> near-add-to-cart">
				<div class="pvt-yith-ywraq-add-button show" style="display:block">

					<a href="#" class="pvt-add-request-quote-button button" data-product_id="<?php echo absint( $product_id ); ?>" data-variation_id="<?php echo absint( $variant_id ); ?>">Ask for a product</a>
					<img src="/wp-content/plugins/yith-woocommerce-request-a-quote-premium/assets/images/wpspin_light.gif" class="ajax-loading" alt="loading" width="16" height="16" style="visibility:hidden">
				</div>
						<div
					class="yith_ywraq_add_item_product-response-<?php echo esc_attr( $variant_id ); ?> yith_ywraq_add_item_product_message hide hide-when-removed"
					style="display:none" data-product_id="<?php echo esc_attr( $variant_id ); ?>"></div>
				<div
					class="yith_ywraq_add_item_response-<?php echo esc_attr( $variant_id ); ?> yith_ywraq_add_item_response_message <?php echo esc_attr( ( ! $exists ) ? 'hide' : 'show' ); ?> hide-when-removed"
					data-product_id="<?php echo esc_attr( $product_id ); ?>"
					style="display:<?php echo ( ! $exists ) ? 'none' : 'block'; ?>"><?php echo esc_html( ywraq_get_label( 'already_in_quote' ) ); ?></div>
				<div
					class="yith_ywraq_add_item_browse-list-<?php echo esc_attr( $variant_id ); ?> yith_ywraq_add_item_browse_message  <?php echo esc_attr( ( ! $exists ) ? 'hide' : 'show' ); ?> hide-when-removed"
					style="display:<?php echo esc_attr( ( ! $exists ) ? 'none' : 'block' ); ?>"
					data-product_id="<?php echo esc_attr( $product_id ); ?>"><a
						href="<?php echo esc_url( $rqa_url ); ?>"><?php echo esc_html( $label_browse ); ?></a></div>
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
				
				$('.pvt-tr div.quantity input.input-text.qty.text').on('change', function(){
					//alert($(this).val());
					$(this).closest('.pvt-tr').find('form.cart input.qty').val($(this).val());
				});

				var xhr = false,
				$widget = $(document).find(ywraq_frontend.widget_classes),
				ajax_loader = (typeof ywraq_frontend !== 'undefined') ? ywraq_frontend.block_loader : false;
				/* Add to cart element */
				$(document).on('click', '.pvt-add-request-quote-button', function(e) {

					e.preventDefault();

					var $t = $(this),
					$t_wrap = $t.closest('.pvt-yith-ywraq-add-to-quote'),
					add_to_cart_info = 'ac',
					$cart_form = '';

					if ($t.hasClass('outofstock')) {
						window.alert(ywraq_frontend.i18n_out_of_stock);
					} else if ($t.hasClass('disabled')) {
						window.alert(ywraq_frontend.i18n_choose_a_variation);
					}
					if ($t.hasClass('disabled') || $t.hasClass('outofstock') || xhr) {
						return;
					}

					if ($('.grouped_form').length) {
						var qtys = 0;
						$('.grouped_form input.qty').each(function() {
							qtys = Math.floor($(this).val()) + qtys;
						});
						if (qtys == 0) {
							alert(ywraq_frontend.select_quantity);
							return;
						}
					}
					// find the form
					if ($t.closest('.cart').length) {
						$cart_form = $t.closest('.cart');
					} else if ($t_wrap.siblings('.cart').first().length) {
						$cart_form = $t_wrap.siblings('.cart').first();
					} else if ($('.composite_form').length) {
						$cart_form = $('.composite_form');
					} else if ($t.closest('ul.products').length > 0) {
						$cart_form = $t.closest('ul.products');
					} else {
						$cart_form = $('form.cart:not(.in_loop)').first(); // not(in_loop) for color and label
					}

					if (typeof $cart_form[0] !== 'undefined' && typeof $cart_form[0].checkValidity === 'function' && !$cart_form[0].checkValidity()) {
						// If the form is invalid, submit it. The form won't actually submit;
						// this will just cause the browser to display the native HTML5 error messages.
						$('<input type="submit">').hide().appendTo($cart_form).click().remove();
						return;
					}

					if ($t.closest('ul.products').length > 0) {
						var $add_to_cart_el = '',
						$product_id_el = $t.closest('li.product').find('a.add_to_cart_button'),
						$product_id_el_val = $product_id_el.data('product_id');
					} else {
						var $add_to_cart_el = $t.closest('.product').find('input[name="add-to-cart"]'),
						$product_id_el = $t.closest('.product').find('input[name="product_id"]'),
						$product_id_el_val = $t.data('product_id') || ($product_id_el.length ? $product_id_el.val() : $add_to_cart_el.val());
					}

					var prod_id = (typeof $product_id_el_val == 'undefined') ? $t.data('product_id') : $product_id_el_val;

					add_to_cart_info = $cart_form.serializefiles();

					add_to_cart_info.append('context', 'frontend');
					add_to_cart_info.append('action', 'yith_ywraq_action');
					add_to_cart_info.append('ywraq_action', 'add_item');
					add_to_cart_info.append('product_id', $t.data('product_id'));
					add_to_cart_info.append('wp_nonce', $t.data('wp_nonce'));
					add_to_cart_info.append('yith-add-to-cart', $t.data('product_id'));
					var quantity = $t_wrap.find('input.qty').val();

					if (quantity > 0) {
						add_to_cart_info.append('quantity', quantity);
					}

					//compatibility with Woocommerce Product Table by Barn2media
					if ($('.wc-product-table-wrapper').length > 0) {
						var quantity = $t.parents('.product-row').find('.cart input.qty').val();
						if (quantity > 0) {
							add_to_cart_info.append('quantity', quantity);
						}

					}

					//compatibility with YITH Quick Order Form
					if ($t.closest('.yith_wc_qof_button_and_price').length > 0) {
						var qof_wrap = $t.closest('.yith_wc_qof_button_and_price'),
						qof_quantity = qof_wrap.find('.YITH_WC_QOF_Quantity_Cart').val();
						add_to_cart_info.append('quantity', qof_quantity);
					}

					// compatibility with color and label
					var wcclForm = $t.closest('li.product').find('.variations_form.in_loop'),
					varID_wccl = wcclForm.length ? wcclForm.data('active_variation') : false;
					if (varID_wccl) {
						add_to_cart_info.append('variation_id', varID_wccl);
						// get select value
						wcclForm.find('select').each(function() {
							add_to_cart_info.append(this.name, this.value);
						});
					}

					$(document).trigger('yith_ywraq_action_before', $t, add_to_cart_info);

					// Easy order page integration ________
					if (typeof yith_wceop_params !== 'undefined' && typeof yith_wceop_params.do_submit !== 'undefined') {
						if (!yith_wceop_params.do_submit) {
							return false;
						}

						if (typeof yith_wceop_params !== 'undefined' && typeof yith_wceop_params.ywraq_quantity !== 'undefined' && yith_wceop_params.ywraq_quantity > 0) {
							add_to_cart_info.append('quantity', yith_wceop_params.ywraq_quantity);
						}

						var variationID = $t.data('variation_id');

						if (variationID) {
							add_to_cart_info.append('variation_id', variationID);
						}

						if ($t.data('variation_args')) {
							var variationArgs = jQuery.parseJSON(decodeURIComponent($t.data('variation_args')));
							for (var key in variationArgs) {
								add_to_cart_info.append(key, variationArgs[key]);
							}
						}
					}

					// Product addons integration ________
					if (typeof yith_wapo_general !== 'undefined') {

						if (!yith_wapo_general.do_submit) {
							return false;
						}

					}

					if (typeof ywcnp_raq !== 'undefined') {
						if (!ywcnp_raq.do_submit) {
							return false;
						}
					}

					xhr = $.ajax({
						type: 'POST',
						url: ywraq_frontend.ajaxurl.toString().replace('%%endpoint%%', 'yith_ywraq_action'),
						dataType: 'json',
						data: add_to_cart_info,
						contentType: false,
						processData: false,
						beforeSend: function() {
							$t.after(' <img src="' + ajax_loader + '" class="ywraq-loader" >');
						},
						complete: function() {
							$t.next().remove();
						},

						success: function(response) {
							if (response.result == 'true' || response.result == 'exists') {
								
								var variationID = $t.data('variation_id');

								if (ywraq_frontend.go_to_the_list == 'yes') {
									window.location.href = response.rqa_url;
								} else { console.log(variationID);
									$t.parent().siblings('.yith_ywraq_add_item_response-' + variationID).hide().addClass('hide').html('');
									$t.parent().siblings('.yith_ywraq_add_item_product-response-' + variationID).show().removeClass('hide').html(response.message);
									$t.parent().siblings('.yith_ywraq_add_item_browse-list-' + variationID).show().removeClass('hide');
									$t.parent().hide().removeClass('show').addClass('addedd');
									$t.closest('.pvt-yith-quote-support').find('input.qty').hide()
									$('.add-to-quote-' + prod_id).attr('data-variation', response.variations);

									if ($widget.length) {
										$widget.ywraq_refresh_widget();
										$widget = $(document).find('.widget_ywraq_list_quote, .widget_ywraq_mini_list_quote');
									}

									ywraq_refresh_number_items();
								}

								$(document).trigger('yith_wwraq_added_successfully', [response, prod_id]);

							} else if (response.result == 'false') {
								$('.yith_ywraq_add_item_response-' + prod_id).show().removeClass('hide').html(response.message);

								$(document).trigger('yith_wwraq_error_while_adding');
							}
							xhr = false;
						},
					});

				});
				
				function ywraq_refresh_number_items() {
					var $number_items = $(document).find('.ywraq_number_items');
					$number_items.each(function() {
					  var $t = $(this),
						  show_url = $t.data('show_url'),
						  item_name = $t.data('item_name'),
						  item_plural_name = $t.data('item_plural_name');

					  $.ajax({
						type: 'POST',
						url: ywraq_frontend.ajaxurl.toString().replace('%%endpoint%%', 'yith_ywraq_action'),
						data: 'ywraq_action=refresh_number_items&action=yith_ywraq_action&context=frontend&item_name=' + item_name + '&item_plural_name=' + item_plural_name + '&show_url=' + show_url,
						success: function(response) {
						  $t.replaceWith(response);
						  $(document).trigger('ywraq_number_items_refreshed');
						},
					  });

					});
				  };


			}(jQuery))

		</script>
	<?php

}, 99 );

// Remove the default YITH Quote button when variable product

add_action('template_redirect', function(){

	if( is_woocommerce() ){
		$adding_to_quote = wc_get_product( get_the_ID() );

		if( $adding_to_quote->is_type( 'variable' ) ){
 			//remove_action( 'woocommerce_single_product_summary', array( YITH_YWRAQ_Frontend::get_instance(), 'add_button_single_page' ), 35 );
			remove_action( 'woocommerce_before_single_product', array( YITH_YWRAQ_Frontend::get_instance(), 'show_button_single_page' ), 0 );
		}
	}

});

// Remove the default cart button with each row
add_filter('pvtfw_disable_add_to_cart_button', '__return_true');
```
### For In-Stock Variation With Add to Cart Button (Premium)
[^ Go to Top](#snippets-for-yith-request-a-quote-premium-plugin)
```
add_filter('pvtfw_row_cart_btn_is', function($default,$product_id, $cart_url, $product_url, $variant_id, $text){
	
	global $product;

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
		$exists = $product->is_type( 'variable' ) ? false : YITH_Request_Quote()->exists( $product_id, $variant_id );
		$rqa_url = YITH_Request_Quote()->get_raq_page_url();
		$label_browse = ywraq_get_label( 'browse_list' );
		

	?>
		<form class="pvt-yith-quote-support cart">
			
			<?php
				$variations = new WC_Product_Variation($variant_id);
				foreach( $variations->get_variation_attributes() as $key => $variation ){
					echo "<input type='hidden' name='".$key."' value='".$variation."'>";
				}
			?>
			
			<input type='hidden' name='quantity' class='qty' value='1'>
			<input type="hidden" name="add-to-cart" value="<?php echo absint($product_id); ?>">
			<input type="hidden" name="product_id" value="<?php echo absint($product_id); ?>">
			<input type="hidden" name="variation_id" class="variation_id" value="<?php echo absint($variant_id); ?>">


			<div class="pvt-yith-ywraq-add-to-quote add-to-quote-<?php echo absint($variant_id); ?> near-add-to-cart">
				<div class="pvt-yith-ywraq-add-button show" style="display:block">

					<a href="#" class="pvt-add-request-quote-button button" data-product_id="<?php echo absint( $product_id ); ?>" data-variation_id="<?php echo absint( $variant_id ); ?>">Ask for a product</a>
					<img src="/wp-content/plugins/yith-woocommerce-request-a-quote-premium/assets/images/wpspin_light.gif" class="ajax-loading" alt="loading" width="16" height="16" style="visibility:hidden">
				</div>
						<div
					class="yith_ywraq_add_item_product-response-<?php echo esc_attr( $variant_id ); ?> yith_ywraq_add_item_product_message hide hide-when-removed"
					style="display:none" data-product_id="<?php echo esc_attr( $variant_id ); ?>"></div>
				<div
					class="yith_ywraq_add_item_response-<?php echo esc_attr( $variant_id ); ?> yith_ywraq_add_item_response_message <?php echo esc_attr( ( ! $exists ) ? 'hide' : 'show' ); ?> hide-when-removed"
					data-product_id="<?php echo esc_attr( $product_id ); ?>"
					style="display:<?php echo ( ! $exists ) ? 'none' : 'block'; ?>"><?php echo esc_html( ywraq_get_label( 'already_in_quote' ) ); ?></div>
				<div
					class="yith_ywraq_add_item_browse-list-<?php echo esc_attr( $variant_id ); ?> yith_ywraq_add_item_browse_message  <?php echo esc_attr( ( ! $exists ) ? 'hide' : 'show' ); ?> hide-when-removed"
					style="display:<?php echo esc_attr( ( ! $exists ) ? 'none' : 'block' ); ?>"
					data-product_id="<?php echo esc_attr( $product_id ); ?>"><a
						href="<?php echo esc_url( $rqa_url ); ?>"><?php echo esc_html( $label_browse ); ?></a></div>
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
				
				$('.pvt-tr div.quantity input.input-text.qty.text').on('change', function(){
					//alert($(this).val());
					$(this).closest('.pvt-tr').find('form.cart input.qty').val($(this).val());
				});

				var xhr = false,
				$widget = $(document).find(ywraq_frontend.widget_classes),
				ajax_loader = (typeof ywraq_frontend !== 'undefined') ? ywraq_frontend.block_loader : false;
				/* Add to cart element */
				$(document).on('click', '.pvt-add-request-quote-button', function(e) {

					e.preventDefault();

					var $t = $(this),
					$t_wrap = $t.closest('.pvt-yith-ywraq-add-to-quote'),
					add_to_cart_info = 'ac',
					$cart_form = '';

					if ($t.hasClass('outofstock')) {
						window.alert(ywraq_frontend.i18n_out_of_stock);
					} else if ($t.hasClass('disabled')) {
						window.alert(ywraq_frontend.i18n_choose_a_variation);
					}
					if ($t.hasClass('disabled') || $t.hasClass('outofstock') || xhr) {
						return;
					}

					if ($('.grouped_form').length) {
						var qtys = 0;
						$('.grouped_form input.qty').each(function() {
							qtys = Math.floor($(this).val()) + qtys;
						});
						if (qtys == 0) {
							alert(ywraq_frontend.select_quantity);
							return;
						}
					}
					// find the form
					if ($t.closest('.cart').length) {
						$cart_form = $t.closest('.cart');
					} else if ($t_wrap.siblings('.cart').first().length) {
						$cart_form = $t_wrap.siblings('.cart').first();
					} else if ($('.composite_form').length) {
						$cart_form = $('.composite_form');
					} else if ($t.closest('ul.products').length > 0) {
						$cart_form = $t.closest('ul.products');
					} else {
						$cart_form = $('form.cart:not(.in_loop)').first(); // not(in_loop) for color and label
					}

					if (typeof $cart_form[0] !== 'undefined' && typeof $cart_form[0].checkValidity === 'function' && !$cart_form[0].checkValidity()) {
						// If the form is invalid, submit it. The form won't actually submit;
						// this will just cause the browser to display the native HTML5 error messages.
						$('<input type="submit">').hide().appendTo($cart_form).click().remove();
						return;
					}

					if ($t.closest('ul.products').length > 0) {
						var $add_to_cart_el = '',
						$product_id_el = $t.closest('li.product').find('a.add_to_cart_button'),
						$product_id_el_val = $product_id_el.data('product_id');
					} else {
						var $add_to_cart_el = $t.closest('.product').find('input[name="add-to-cart"]'),
						$product_id_el = $t.closest('.product').find('input[name="product_id"]'),
						$product_id_el_val = $t.data('product_id') || ($product_id_el.length ? $product_id_el.val() : $add_to_cart_el.val());
					}

					var prod_id = (typeof $product_id_el_val == 'undefined') ? $t.data('product_id') : $product_id_el_val;

					add_to_cart_info = $cart_form.serializefiles();

					add_to_cart_info.append('context', 'frontend');
					add_to_cart_info.append('action', 'yith_ywraq_action');
					add_to_cart_info.append('ywraq_action', 'add_item');
					add_to_cart_info.append('product_id', $t.data('product_id'));
					add_to_cart_info.append('wp_nonce', $t.data('wp_nonce'));
					add_to_cart_info.append('yith-add-to-cart', $t.data('product_id'));
					var quantity = $t_wrap.find('input.qty').val();

					if (quantity > 0) {
						add_to_cart_info.append('quantity', quantity);
					}

					//compatibility with Woocommerce Product Table by Barn2media
					if ($('.wc-product-table-wrapper').length > 0) {
						var quantity = $t.parents('.product-row').find('.cart input.qty').val();
						if (quantity > 0) {
							add_to_cart_info.append('quantity', quantity);
						}

					}

					//compatibility with YITH Quick Order Form
					if ($t.closest('.yith_wc_qof_button_and_price').length > 0) {
						var qof_wrap = $t.closest('.yith_wc_qof_button_and_price'),
						qof_quantity = qof_wrap.find('.YITH_WC_QOF_Quantity_Cart').val();
						add_to_cart_info.append('quantity', qof_quantity);
					}

					// compatibility with color and label
					var wcclForm = $t.closest('li.product').find('.variations_form.in_loop'),
					varID_wccl = wcclForm.length ? wcclForm.data('active_variation') : false;
					if (varID_wccl) {
						add_to_cart_info.append('variation_id', varID_wccl);
						// get select value
						wcclForm.find('select').each(function() {
							add_to_cart_info.append(this.name, this.value);
						});
					}

					$(document).trigger('yith_ywraq_action_before', $t, add_to_cart_info);

					// Easy order page integration ________
					if (typeof yith_wceop_params !== 'undefined' && typeof yith_wceop_params.do_submit !== 'undefined') {
						if (!yith_wceop_params.do_submit) {
							return false;
						}

						if (typeof yith_wceop_params !== 'undefined' && typeof yith_wceop_params.ywraq_quantity !== 'undefined' && yith_wceop_params.ywraq_quantity > 0) {
							add_to_cart_info.append('quantity', yith_wceop_params.ywraq_quantity);
						}

						var variationID = $t.data('variation_id');

						if (variationID) {
							add_to_cart_info.append('variation_id', variationID);
						}

						if ($t.data('variation_args')) {
							var variationArgs = jQuery.parseJSON(decodeURIComponent($t.data('variation_args')));
							for (var key in variationArgs) {
								add_to_cart_info.append(key, variationArgs[key]);
							}
						}
					}

					// Product addons integration ________
					if (typeof yith_wapo_general !== 'undefined') {

						if (!yith_wapo_general.do_submit) {
							return false;
						}

					}

					if (typeof ywcnp_raq !== 'undefined') {
						if (!ywcnp_raq.do_submit) {
							return false;
						}
					}

					xhr = $.ajax({
						type: 'POST',
						url: ywraq_frontend.ajaxurl.toString().replace('%%endpoint%%', 'yith_ywraq_action'),
						dataType: 'json',
						data: add_to_cart_info,
						contentType: false,
						processData: false,
						beforeSend: function() {
							$t.after(' <img src="' + ajax_loader + '" class="ywraq-loader" >');
						},
						complete: function() {
							$t.next().remove();
						},

						success: function(response) {
							if (response.result == 'true' || response.result == 'exists') {
								
								var variationID = $t.data('variation_id');

								if (ywraq_frontend.go_to_the_list == 'yes') {
									window.location.href = response.rqa_url;
								} else { console.log(variationID);
									$t.parent().siblings('.yith_ywraq_add_item_response-' + variationID).hide().addClass('hide').html('');
									$t.parent().siblings('.yith_ywraq_add_item_product-response-' + variationID).show().removeClass('hide').html(response.message);
									$t.parent().siblings('.yith_ywraq_add_item_browse-list-' + variationID).show().removeClass('hide');
									$t.parent().hide().removeClass('show').addClass('addedd');
									$t.closest('.pvt-yith-quote-support').find('input.qty').hide()
									$('.add-to-quote-' + prod_id).attr('data-variation', response.variations);

									if ($widget.length) {
										$widget.ywraq_refresh_widget();
										$widget = $(document).find('.widget_ywraq_list_quote, .widget_ywraq_mini_list_quote');
									}

									ywraq_refresh_number_items();
								}

								$(document).trigger('yith_wwraq_added_successfully', [response, prod_id]);

							} else if (response.result == 'false') {
								$('.yith_ywraq_add_item_response-' + prod_id).show().removeClass('hide').html(response.message);

								$(document).trigger('yith_wwraq_error_while_adding');
							}
							xhr = false;
						},
					});

				});
				
				function ywraq_refresh_number_items() {
					var $number_items = $(document).find('.ywraq_number_items');
					$number_items.each(function() {
					  var $t = $(this),
						  show_url = $t.data('show_url'),
						  item_name = $t.data('item_name'),
						  item_plural_name = $t.data('item_plural_name');

					  $.ajax({
						type: 'POST',
						url: ywraq_frontend.ajaxurl.toString().replace('%%endpoint%%', 'yith_ywraq_action'),
						data: 'ywraq_action=refresh_number_items&action=yith_ywraq_action&context=frontend&item_name=' + item_name + '&item_plural_name=' + item_plural_name + '&show_url=' + show_url,
						success: function(response) {
						  $t.replaceWith(response);
						  $(document).trigger('ywraq_number_items_refreshed');
						},
					  });

					});
				  };


			}(jQuery))

		</script>
	<?php

}, 99 );

// Remove the default YITH Quote button when variable product

add_action('template_redirect', function(){

	if( is_woocommerce() ){
		$adding_to_quote = wc_get_product( get_the_ID() );

		if( $adding_to_quote->is_type( 'variable' ) ){
 			//remove_action( 'woocommerce_single_product_summary', array( YITH_YWRAQ_Frontend::get_instance(), 'add_button_single_page' ), 35 );
			remove_action( 'woocommerce_before_single_product', array( YITH_YWRAQ_Frontend::get_instance(), 'show_button_single_page' ), 0 );
		}
	}

});
```
### For Out-of-Stock Variation Without Out-of-stock Button (Premium)
[^ Go to Top](#snippets-for-yith-request-a-quote-premium-plugin)
```
add_filter('pvtfw_row_cart_btn_oos', function($default,$product_id, $cart_url, $product_url, $variant_id, $text){
	
	global $product;

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
		$exists = $product->is_type( 'variable' ) ? false : YITH_Request_Quote()->exists( $product_id, $variant_id );
		$rqa_url = YITH_Request_Quote()->get_raq_page_url();
		$label_browse = ywraq_get_label( 'browse_list' );
		

	?>
		<form class="pvt-yith-quote-support cart">
			
			<?php
				$variations = new WC_Product_Variation($variant_id);
				foreach( $variations->get_variation_attributes() as $key => $variation ){
					echo "<input type='hidden' name='".$key."' value='".$variation."'>";
				}
			?>
			
			<input type='hidden' name='quantity' class='qty' value='1'>
			<input type="hidden" name="add-to-cart" value="<?php echo absint($product_id); ?>">
			<input type="hidden" name="product_id" value="<?php echo absint($product_id); ?>">
			<input type="hidden" name="variation_id" class="variation_id" value="<?php echo absint($variant_id); ?>">


			<div class="pvt-yith-ywraq-add-to-quote add-to-quote-<?php echo absint($variant_id); ?> near-add-to-cart">
				<div class="pvt-yith-ywraq-add-button show" style="display:block">

					<a href="#" class="pvt-add-request-quote-button button" data-product_id="<?php echo absint( $product_id ); ?>" data-variation_id="<?php echo absint( $variant_id ); ?>">Ask for a product</a>
					<img src="/wp-content/plugins/yith-woocommerce-request-a-quote-premium/assets/images/wpspin_light.gif" class="ajax-loading" alt="loading" width="16" height="16" style="visibility:hidden">
				</div>
						<div
					class="yith_ywraq_add_item_product-response-<?php echo esc_attr( $variant_id ); ?> yith_ywraq_add_item_product_message hide hide-when-removed"
					style="display:none" data-product_id="<?php echo esc_attr( $variant_id ); ?>"></div>
				<div
					class="yith_ywraq_add_item_response-<?php echo esc_attr( $variant_id ); ?> yith_ywraq_add_item_response_message <?php echo esc_attr( ( ! $exists ) ? 'hide' : 'show' ); ?> hide-when-removed"
					data-product_id="<?php echo esc_attr( $product_id ); ?>"
					style="display:<?php echo ( ! $exists ) ? 'none' : 'block'; ?>"><?php echo esc_html( ywraq_get_label( 'already_in_quote' ) ); ?></div>
				<div
					class="yith_ywraq_add_item_browse-list-<?php echo esc_attr( $variant_id ); ?> yith_ywraq_add_item_browse_message  <?php echo esc_attr( ( ! $exists ) ? 'hide' : 'show' ); ?> hide-when-removed"
					style="display:<?php echo esc_attr( ( ! $exists ) ? 'none' : 'block' ); ?>"
					data-product_id="<?php echo esc_attr( $product_id ); ?>"><a
						href="<?php echo esc_url( $rqa_url ); ?>"><?php echo esc_html( $label_browse ); ?></a></div>
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
				
				$('.pvt-tr div.quantity input.input-text.qty.text').on('change', function(){
					//alert($(this).val());
					$(this).closest('.pvt-tr').find('form.cart input.qty').val($(this).val());
				});

				var xhr = false,
				$widget = $(document).find(ywraq_frontend.widget_classes),
				ajax_loader = (typeof ywraq_frontend !== 'undefined') ? ywraq_frontend.block_loader : false;
				/* Add to cart element */
				$(document).on('click', '.pvt-add-request-quote-button', function(e) {

					e.preventDefault();

					var $t = $(this),
					$t_wrap = $t.closest('.pvt-yith-ywraq-add-to-quote'),
					add_to_cart_info = 'ac',
					$cart_form = '';

					if ($t.hasClass('outofstock')) {
						window.alert(ywraq_frontend.i18n_out_of_stock);
					} else if ($t.hasClass('disabled')) {
						window.alert(ywraq_frontend.i18n_choose_a_variation);
					}
					if ($t.hasClass('disabled') || $t.hasClass('outofstock') || xhr) {
						return;
					}

					if ($('.grouped_form').length) {
						var qtys = 0;
						$('.grouped_form input.qty').each(function() {
							qtys = Math.floor($(this).val()) + qtys;
						});
						if (qtys == 0) {
							alert(ywraq_frontend.select_quantity);
							return;
						}
					}
					// find the form
					if ($t.closest('.cart').length) {
						$cart_form = $t.closest('.cart');
					} else if ($t_wrap.siblings('.cart').first().length) {
						$cart_form = $t_wrap.siblings('.cart').first();
					} else if ($('.composite_form').length) {
						$cart_form = $('.composite_form');
					} else if ($t.closest('ul.products').length > 0) {
						$cart_form = $t.closest('ul.products');
					} else {
						$cart_form = $('form.cart:not(.in_loop)').first(); // not(in_loop) for color and label
					}

					if (typeof $cart_form[0] !== 'undefined' && typeof $cart_form[0].checkValidity === 'function' && !$cart_form[0].checkValidity()) {
						// If the form is invalid, submit it. The form won't actually submit;
						// this will just cause the browser to display the native HTML5 error messages.
						$('<input type="submit">').hide().appendTo($cart_form).click().remove();
						return;
					}

					if ($t.closest('ul.products').length > 0) {
						var $add_to_cart_el = '',
						$product_id_el = $t.closest('li.product').find('a.add_to_cart_button'),
						$product_id_el_val = $product_id_el.data('product_id');
					} else {
						var $add_to_cart_el = $t.closest('.product').find('input[name="add-to-cart"]'),
						$product_id_el = $t.closest('.product').find('input[name="product_id"]'),
						$product_id_el_val = $t.data('product_id') || ($product_id_el.length ? $product_id_el.val() : $add_to_cart_el.val());
					}

					var prod_id = (typeof $product_id_el_val == 'undefined') ? $t.data('product_id') : $product_id_el_val;

					add_to_cart_info = $cart_form.serializefiles();

					add_to_cart_info.append('context', 'frontend');
					add_to_cart_info.append('action', 'yith_ywraq_action');
					add_to_cart_info.append('ywraq_action', 'add_item');
					add_to_cart_info.append('product_id', $t.data('product_id'));
					add_to_cart_info.append('wp_nonce', $t.data('wp_nonce'));
					add_to_cart_info.append('yith-add-to-cart', $t.data('product_id'));
					var quantity = $t_wrap.find('input.qty').val();

					if (quantity > 0) {
						add_to_cart_info.append('quantity', quantity);
					}

					//compatibility with Woocommerce Product Table by Barn2media
					if ($('.wc-product-table-wrapper').length > 0) {
						var quantity = $t.parents('.product-row').find('.cart input.qty').val();
						if (quantity > 0) {
							add_to_cart_info.append('quantity', quantity);
						}

					}

					//compatibility with YITH Quick Order Form
					if ($t.closest('.yith_wc_qof_button_and_price').length > 0) {
						var qof_wrap = $t.closest('.yith_wc_qof_button_and_price'),
						qof_quantity = qof_wrap.find('.YITH_WC_QOF_Quantity_Cart').val();
						add_to_cart_info.append('quantity', qof_quantity);
					}

					// compatibility with color and label
					var wcclForm = $t.closest('li.product').find('.variations_form.in_loop'),
					varID_wccl = wcclForm.length ? wcclForm.data('active_variation') : false;
					if (varID_wccl) {
						add_to_cart_info.append('variation_id', varID_wccl);
						// get select value
						wcclForm.find('select').each(function() {
							add_to_cart_info.append(this.name, this.value);
						});
					}

					$(document).trigger('yith_ywraq_action_before', $t, add_to_cart_info);

					// Easy order page integration ________
					if (typeof yith_wceop_params !== 'undefined' && typeof yith_wceop_params.do_submit !== 'undefined') {
						if (!yith_wceop_params.do_submit) {
							return false;
						}

						if (typeof yith_wceop_params !== 'undefined' && typeof yith_wceop_params.ywraq_quantity !== 'undefined' && yith_wceop_params.ywraq_quantity > 0) {
							add_to_cart_info.append('quantity', yith_wceop_params.ywraq_quantity);
						}

						var variationID = $t.data('variation_id');

						if (variationID) {
							add_to_cart_info.append('variation_id', variationID);
						}

						if ($t.data('variation_args')) {
							var variationArgs = jQuery.parseJSON(decodeURIComponent($t.data('variation_args')));
							for (var key in variationArgs) {
								add_to_cart_info.append(key, variationArgs[key]);
							}
						}
					}

					// Product addons integration ________
					if (typeof yith_wapo_general !== 'undefined') {

						if (!yith_wapo_general.do_submit) {
							return false;
						}

					}

					if (typeof ywcnp_raq !== 'undefined') {
						if (!ywcnp_raq.do_submit) {
							return false;
						}
					}

					xhr = $.ajax({
						type: 'POST',
						url: ywraq_frontend.ajaxurl.toString().replace('%%endpoint%%', 'yith_ywraq_action'),
						dataType: 'json',
						data: add_to_cart_info,
						contentType: false,
						processData: false,
						beforeSend: function() {
							$t.after(' <img src="' + ajax_loader + '" class="ywraq-loader" >');
						},
						complete: function() {
							$t.next().remove();
						},

						success: function(response) {
							if (response.result == 'true' || response.result == 'exists') {
								
								var variationID = $t.data('variation_id');

								if (ywraq_frontend.go_to_the_list == 'yes') {
									window.location.href = response.rqa_url;
								} else { console.log(variationID);
									$t.parent().siblings('.yith_ywraq_add_item_response-' + variationID).hide().addClass('hide').html('');
									$t.parent().siblings('.yith_ywraq_add_item_product-response-' + variationID).show().removeClass('hide').html(response.message);
									$t.parent().siblings('.yith_ywraq_add_item_browse-list-' + variationID).show().removeClass('hide');
									$t.parent().hide().removeClass('show').addClass('addedd');
									$t.closest('.pvt-yith-quote-support').find('input.qty').hide()
									$('.add-to-quote-' + prod_id).attr('data-variation', response.variations);

									if ($widget.length) {
										$widget.ywraq_refresh_widget();
										$widget = $(document).find('.widget_ywraq_list_quote, .widget_ywraq_mini_list_quote');
									}

									ywraq_refresh_number_items();
								}

								$(document).trigger('yith_wwraq_added_successfully', [response, prod_id]);

							} else if (response.result == 'false') {
								$('.yith_ywraq_add_item_response-' + prod_id).show().removeClass('hide').html(response.message);

								$(document).trigger('yith_wwraq_error_while_adding');
							}
							xhr = false;
						},
					});

				});
				
				function ywraq_refresh_number_items() {
					var $number_items = $(document).find('.ywraq_number_items');
					$number_items.each(function() {
					  var $t = $(this),
						  show_url = $t.data('show_url'),
						  item_name = $t.data('item_name'),
						  item_plural_name = $t.data('item_plural_name');

					  $.ajax({
						type: 'POST',
						url: ywraq_frontend.ajaxurl.toString().replace('%%endpoint%%', 'yith_ywraq_action'),
						data: 'ywraq_action=refresh_number_items&action=yith_ywraq_action&context=frontend&item_name=' + item_name + '&item_plural_name=' + item_plural_name + '&show_url=' + show_url,
						success: function(response) {
						  $t.replaceWith(response);
						  $(document).trigger('ywraq_number_items_refreshed');
						},
					  });

					});
				  };


			}(jQuery))

		</script>
	<?php

}, 99 );

// Remove the default YITH Quote button when variable product

add_action('template_redirect', function(){

	if( is_woocommerce() ){
		$adding_to_quote = wc_get_product( get_the_ID() );

		if( $adding_to_quote->is_type( 'variable' ) ){
 			//remove_action( 'woocommerce_single_product_summary', array( YITH_YWRAQ_Frontend::get_instance(), 'add_button_single_page' ), 35 );
			remove_action( 'woocommerce_before_single_product', array( YITH_YWRAQ_Frontend::get_instance(), 'show_button_single_page' ), 0 );
		}
	}

});

// Remove the default out-of-stock button with each row
add_filter('pvtfw_disable_out_of_stock_button', '__return_true');
```
### For Out-of-Stock Variation With Out-of-Stock Button (Premium)
[^ Go to Top](#snippets-for-yith-request-a-quote-premium-plugin)
```
add_filter('pvtfw_row_cart_btn_oos', function($default,$product_id, $cart_url, $product_url, $variant_id, $text){
	
	global $product;

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
		$exists = $product->is_type( 'variable' ) ? false : YITH_Request_Quote()->exists( $product_id, $variant_id );
		$rqa_url = YITH_Request_Quote()->get_raq_page_url();
		$label_browse = ywraq_get_label( 'browse_list' );
		

	?>
		<form class="pvt-yith-quote-support cart">
			
			<?php
				$variations = new WC_Product_Variation($variant_id);
				foreach( $variations->get_variation_attributes() as $key => $variation ){
					echo "<input type='hidden' name='".$key."' value='".$variation."'>";
				}
			?>
			
			<input type='hidden' name='quantity' class='qty' value='1'>
			<input type="hidden" name="add-to-cart" value="<?php echo absint($product_id); ?>">
			<input type="hidden" name="product_id" value="<?php echo absint($product_id); ?>">
			<input type="hidden" name="variation_id" class="variation_id" value="<?php echo absint($variant_id); ?>">


			<div class="pvt-yith-ywraq-add-to-quote add-to-quote-<?php echo absint($variant_id); ?> near-add-to-cart">
				<div class="pvt-yith-ywraq-add-button show" style="display:block">

					<a href="#" class="pvt-add-request-quote-button button" data-product_id="<?php echo absint( $product_id ); ?>" data-variation_id="<?php echo absint( $variant_id ); ?>">Ask for a product</a>
					<img src="/wp-content/plugins/yith-woocommerce-request-a-quote-premium/assets/images/wpspin_light.gif" class="ajax-loading" alt="loading" width="16" height="16" style="visibility:hidden">
				</div>
						<div
					class="yith_ywraq_add_item_product-response-<?php echo esc_attr( $variant_id ); ?> yith_ywraq_add_item_product_message hide hide-when-removed"
					style="display:none" data-product_id="<?php echo esc_attr( $variant_id ); ?>"></div>
				<div
					class="yith_ywraq_add_item_response-<?php echo esc_attr( $variant_id ); ?> yith_ywraq_add_item_response_message <?php echo esc_attr( ( ! $exists ) ? 'hide' : 'show' ); ?> hide-when-removed"
					data-product_id="<?php echo esc_attr( $product_id ); ?>"
					style="display:<?php echo ( ! $exists ) ? 'none' : 'block'; ?>"><?php echo esc_html( ywraq_get_label( 'already_in_quote' ) ); ?></div>
				<div
					class="yith_ywraq_add_item_browse-list-<?php echo esc_attr( $variant_id ); ?> yith_ywraq_add_item_browse_message  <?php echo esc_attr( ( ! $exists ) ? 'hide' : 'show' ); ?> hide-when-removed"
					style="display:<?php echo esc_attr( ( ! $exists ) ? 'none' : 'block' ); ?>"
					data-product_id="<?php echo esc_attr( $product_id ); ?>"><a
						href="<?php echo esc_url( $rqa_url ); ?>"><?php echo esc_html( $label_browse ); ?></a></div>
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
				
				$('.pvt-tr div.quantity input.input-text.qty.text').on('change', function(){
					//alert($(this).val());
					$(this).closest('.pvt-tr').find('form.cart input.qty').val($(this).val());
				});

				var xhr = false,
				$widget = $(document).find(ywraq_frontend.widget_classes),
				ajax_loader = (typeof ywraq_frontend !== 'undefined') ? ywraq_frontend.block_loader : false;
				/* Add to cart element */
				$(document).on('click', '.pvt-add-request-quote-button', function(e) {

					e.preventDefault();

					var $t = $(this),
					$t_wrap = $t.closest('.pvt-yith-ywraq-add-to-quote'),
					add_to_cart_info = 'ac',
					$cart_form = '';

					if ($t.hasClass('outofstock')) {
						window.alert(ywraq_frontend.i18n_out_of_stock);
					} else if ($t.hasClass('disabled')) {
						window.alert(ywraq_frontend.i18n_choose_a_variation);
					}
					if ($t.hasClass('disabled') || $t.hasClass('outofstock') || xhr) {
						return;
					}

					if ($('.grouped_form').length) {
						var qtys = 0;
						$('.grouped_form input.qty').each(function() {
							qtys = Math.floor($(this).val()) + qtys;
						});
						if (qtys == 0) {
							alert(ywraq_frontend.select_quantity);
							return;
						}
					}
					// find the form
					if ($t.closest('.cart').length) {
						$cart_form = $t.closest('.cart');
					} else if ($t_wrap.siblings('.cart').first().length) {
						$cart_form = $t_wrap.siblings('.cart').first();
					} else if ($('.composite_form').length) {
						$cart_form = $('.composite_form');
					} else if ($t.closest('ul.products').length > 0) {
						$cart_form = $t.closest('ul.products');
					} else {
						$cart_form = $('form.cart:not(.in_loop)').first(); // not(in_loop) for color and label
					}

					if (typeof $cart_form[0] !== 'undefined' && typeof $cart_form[0].checkValidity === 'function' && !$cart_form[0].checkValidity()) {
						// If the form is invalid, submit it. The form won't actually submit;
						// this will just cause the browser to display the native HTML5 error messages.
						$('<input type="submit">').hide().appendTo($cart_form).click().remove();
						return;
					}

					if ($t.closest('ul.products').length > 0) {
						var $add_to_cart_el = '',
						$product_id_el = $t.closest('li.product').find('a.add_to_cart_button'),
						$product_id_el_val = $product_id_el.data('product_id');
					} else {
						var $add_to_cart_el = $t.closest('.product').find('input[name="add-to-cart"]'),
						$product_id_el = $t.closest('.product').find('input[name="product_id"]'),
						$product_id_el_val = $t.data('product_id') || ($product_id_el.length ? $product_id_el.val() : $add_to_cart_el.val());
					}

					var prod_id = (typeof $product_id_el_val == 'undefined') ? $t.data('product_id') : $product_id_el_val;

					add_to_cart_info = $cart_form.serializefiles();

					add_to_cart_info.append('context', 'frontend');
					add_to_cart_info.append('action', 'yith_ywraq_action');
					add_to_cart_info.append('ywraq_action', 'add_item');
					add_to_cart_info.append('product_id', $t.data('product_id'));
					add_to_cart_info.append('wp_nonce', $t.data('wp_nonce'));
					add_to_cart_info.append('yith-add-to-cart', $t.data('product_id'));
					var quantity = $t_wrap.find('input.qty').val();

					if (quantity > 0) {
						add_to_cart_info.append('quantity', quantity);
					}

					//compatibility with Woocommerce Product Table by Barn2media
					if ($('.wc-product-table-wrapper').length > 0) {
						var quantity = $t.parents('.product-row').find('.cart input.qty').val();
						if (quantity > 0) {
							add_to_cart_info.append('quantity', quantity);
						}

					}

					//compatibility with YITH Quick Order Form
					if ($t.closest('.yith_wc_qof_button_and_price').length > 0) {
						var qof_wrap = $t.closest('.yith_wc_qof_button_and_price'),
						qof_quantity = qof_wrap.find('.YITH_WC_QOF_Quantity_Cart').val();
						add_to_cart_info.append('quantity', qof_quantity);
					}

					// compatibility with color and label
					var wcclForm = $t.closest('li.product').find('.variations_form.in_loop'),
					varID_wccl = wcclForm.length ? wcclForm.data('active_variation') : false;
					if (varID_wccl) {
						add_to_cart_info.append('variation_id', varID_wccl);
						// get select value
						wcclForm.find('select').each(function() {
							add_to_cart_info.append(this.name, this.value);
						});
					}

					$(document).trigger('yith_ywraq_action_before', $t, add_to_cart_info);

					// Easy order page integration ________
					if (typeof yith_wceop_params !== 'undefined' && typeof yith_wceop_params.do_submit !== 'undefined') {
						if (!yith_wceop_params.do_submit) {
							return false;
						}

						if (typeof yith_wceop_params !== 'undefined' && typeof yith_wceop_params.ywraq_quantity !== 'undefined' && yith_wceop_params.ywraq_quantity > 0) {
							add_to_cart_info.append('quantity', yith_wceop_params.ywraq_quantity);
						}

						var variationID = $t.data('variation_id');

						if (variationID) {
							add_to_cart_info.append('variation_id', variationID);
						}

						if ($t.data('variation_args')) {
							var variationArgs = jQuery.parseJSON(decodeURIComponent($t.data('variation_args')));
							for (var key in variationArgs) {
								add_to_cart_info.append(key, variationArgs[key]);
							}
						}
					}

					// Product addons integration ________
					if (typeof yith_wapo_general !== 'undefined') {

						if (!yith_wapo_general.do_submit) {
							return false;
						}

					}

					if (typeof ywcnp_raq !== 'undefined') {
						if (!ywcnp_raq.do_submit) {
							return false;
						}
					}

					xhr = $.ajax({
						type: 'POST',
						url: ywraq_frontend.ajaxurl.toString().replace('%%endpoint%%', 'yith_ywraq_action'),
						dataType: 'json',
						data: add_to_cart_info,
						contentType: false,
						processData: false,
						beforeSend: function() {
							$t.after(' <img src="' + ajax_loader + '" class="ywraq-loader" >');
						},
						complete: function() {
							$t.next().remove();
						},

						success: function(response) {
							if (response.result == 'true' || response.result == 'exists') {
								
								var variationID = $t.data('variation_id');

								if (ywraq_frontend.go_to_the_list == 'yes') {
									window.location.href = response.rqa_url;
								} else { console.log(variationID);
									$t.parent().siblings('.yith_ywraq_add_item_response-' + variationID).hide().addClass('hide').html('');
									$t.parent().siblings('.yith_ywraq_add_item_product-response-' + variationID).show().removeClass('hide').html(response.message);
									$t.parent().siblings('.yith_ywraq_add_item_browse-list-' + variationID).show().removeClass('hide');
									$t.parent().hide().removeClass('show').addClass('addedd');
									$t.closest('.pvt-yith-quote-support').find('input.qty').hide()
									$('.add-to-quote-' + prod_id).attr('data-variation', response.variations);

									if ($widget.length) {
										$widget.ywraq_refresh_widget();
										$widget = $(document).find('.widget_ywraq_list_quote, .widget_ywraq_mini_list_quote');
									}

									ywraq_refresh_number_items();
								}

								$(document).trigger('yith_wwraq_added_successfully', [response, prod_id]);

							} else if (response.result == 'false') {
								$('.yith_ywraq_add_item_response-' + prod_id).show().removeClass('hide').html(response.message);

								$(document).trigger('yith_wwraq_error_while_adding');
							}
							xhr = false;
						},
					});

				});
				
				function ywraq_refresh_number_items() {
					var $number_items = $(document).find('.ywraq_number_items');
					$number_items.each(function() {
					  var $t = $(this),
						  show_url = $t.data('show_url'),
						  item_name = $t.data('item_name'),
						  item_plural_name = $t.data('item_plural_name');

					  $.ajax({
						type: 'POST',
						url: ywraq_frontend.ajaxurl.toString().replace('%%endpoint%%', 'yith_ywraq_action'),
						data: 'ywraq_action=refresh_number_items&action=yith_ywraq_action&context=frontend&item_name=' + item_name + '&item_plural_name=' + item_plural_name + '&show_url=' + show_url,
						success: function(response) {
						  $t.replaceWith(response);
						  $(document).trigger('ywraq_number_items_refreshed');
						},
					  });

					});
				  };


			}(jQuery))

		</script>
	<?php

}, 99 );

// Remove the default YITH Quote button when variable product

add_action('template_redirect', function(){

	if( is_woocommerce() ){
		$adding_to_quote = wc_get_product( get_the_ID() );

		if( $adding_to_quote->is_type( 'variable' ) ){
 			//remove_action( 'woocommerce_single_product_summary', array( YITH_YWRAQ_Frontend::get_instance(), 'add_button_single_page' ), 35 );
			remove_action( 'woocommerce_before_single_product', array( YITH_YWRAQ_Frontend::get_instance(), 'show_button_single_page' ), 0 );
		}
	}

});
```
### Role base quote button

> [!IMPORTANT]
> - You can also use it for the *In-stock variation* as well. Just replace `pvtfw_row_cart_btn_oos` with `pvtfw_row_cart_btn_is`
> - For *Out-of-stock variation* after the snippet add this `add_filter('pvtfw_disable_out_of_stock_button', '__return_true')` to disable the Out-of-stock button.
> - For *In_stock variation* after the snippet add this `add_filter('pvtfw_disable_add_to_cart_button', '__return_true')` to disable the Add to cart button.

```
// PVT filtering Out of Stock Button
add_filter('pvtfw_row_cart_btn_oos', function($default,$product_id, $cart_url, $product_url, $variant_id, $text){
	
	global $product;

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
		$exists = $product->is_type( 'variable' ) ? false : YITH_Request_Quote()->exists( $product_id, $variant_id );
		$rqa_url = YITH_Request_Quote()->get_raq_page_url();
		$label_browse = ywraq_get_label( 'browse_list' );
		

	?>
		<form class="pvt-yith-quote-support cart">
			
			<?php
				$variations = new WC_Product_Variation($variant_id);
				foreach( $variations->get_variation_attributes() as $key => $variation ){
					echo "<input type='hidden' name='".$key."' value='".$variation."'>";
				}
			?>
			
			<input type='hidden' name='quantity' class='qty' value='1'>
			<input type="hidden" name="add-to-cart" value="<?php echo absint($product_id); ?>">
			<input type="hidden" name="product_id" value="<?php echo absint($product_id); ?>">
			<input type="hidden" name="variation_id" class="variation_id" value="<?php echo absint($variant_id); ?>">


			<div class="pvt-yith-ywraq-add-to-quote add-to-quote-<?php echo absint($variant_id); ?> near-add-to-cart">
				<div class="pvt-yith-ywraq-add-button show" style="display:block">

					<a href="#" class="pvt-add-request-quote-button button" data-product_id="<?php echo absint( $product_id ); ?>" data-variation_id="<?php echo absint( $variant_id ); ?>">Ask for a product</a>
					<img src="/wp-content/plugins/yith-woocommerce-request-a-quote-premium/assets/images/wpspin_light.gif" class="ajax-loading" alt="loading" width="16" height="16" style="visibility:hidden">
				</div>
						<div
					class="yith_ywraq_add_item_product-response-<?php echo esc_attr( $variant_id ); ?> yith_ywraq_add_item_product_message hide hide-when-removed"
					style="display:none" data-product_id="<?php echo esc_attr( $variant_id ); ?>"></div>
				<div
					class="yith_ywraq_add_item_response-<?php echo esc_attr( $variant_id ); ?> yith_ywraq_add_item_response_message <?php echo esc_attr( ( ! $exists ) ? 'hide' : 'show' ); ?> hide-when-removed"
					data-product_id="<?php echo esc_attr( $product_id ); ?>"
					style="display:<?php echo ( ! $exists ) ? 'none' : 'block'; ?>"><?php echo esc_html( ywraq_get_label( 'already_in_quote' ) ); ?></div>
				<div
					class="yith_ywraq_add_item_browse-list-<?php echo esc_attr( $variant_id ); ?> yith_ywraq_add_item_browse_message  <?php echo esc_attr( ( ! $exists ) ? 'hide' : 'show' ); ?> hide-when-removed"
					style="display:<?php echo esc_attr( ( ! $exists ) ? 'none' : 'block' ); ?>"
					data-product_id="<?php echo esc_attr( $product_id ); ?>"><a
						href="<?php echo esc_url( $rqa_url ); ?>"><?php echo esc_html( $label_browse ); ?></a></div>
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
				
				$('.pvt-tr div.quantity input.input-text.qty.text').on('change', function(){
					//alert($(this).val());
					$(this).closest('.pvt-tr').find('form.cart input.qty').val($(this).val());
				});

				var xhr = false,
				$widget = $(document).find(ywraq_frontend.widget_classes),
				ajax_loader = (typeof ywraq_frontend !== 'undefined') ? ywraq_frontend.block_loader : false;
				/* Add to cart element */
				$(document).on('click', '.pvt-add-request-quote-button', function(e) {

					e.preventDefault();

					var $t = $(this),
					$t_wrap = $t.closest('.pvt-yith-ywraq-add-to-quote'),
					add_to_cart_info = 'ac',
					$cart_form = '';

					if ($t.hasClass('outofstock')) {
						window.alert(ywraq_frontend.i18n_out_of_stock);
					} else if ($t.hasClass('disabled')) {
						window.alert(ywraq_frontend.i18n_choose_a_variation);
					}
					if ($t.hasClass('disabled') || $t.hasClass('outofstock') || xhr) {
						return;
					}

					if ($('.grouped_form').length) {
						var qtys = 0;
						$('.grouped_form input.qty').each(function() {
							qtys = Math.floor($(this).val()) + qtys;
						});
						if (qtys == 0) {
							alert(ywraq_frontend.select_quantity);
							return;
						}
					}
					// find the form
					if ($t.closest('.cart').length) {
						$cart_form = $t.closest('.cart');
					} else if ($t_wrap.siblings('.cart').first().length) {
						$cart_form = $t_wrap.siblings('.cart').first();
					} else if ($('.composite_form').length) {
						$cart_form = $('.composite_form');
					} else if ($t.closest('ul.products').length > 0) {
						$cart_form = $t.closest('ul.products');
					} else {
						$cart_form = $('form.cart:not(.in_loop)').first(); // not(in_loop) for color and label
					}

					if (typeof $cart_form[0] !== 'undefined' && typeof $cart_form[0].checkValidity === 'function' && !$cart_form[0].checkValidity()) {
						// If the form is invalid, submit it. The form won't actually submit;
						// this will just cause the browser to display the native HTML5 error messages.
						$('<input type="submit">').hide().appendTo($cart_form).click().remove();
						return;
					}

					if ($t.closest('ul.products').length > 0) {
						var $add_to_cart_el = '',
						$product_id_el = $t.closest('li.product').find('a.add_to_cart_button'),
						$product_id_el_val = $product_id_el.data('product_id');
					} else {
						var $add_to_cart_el = $t.closest('.product').find('input[name="add-to-cart"]'),
						$product_id_el = $t.closest('.product').find('input[name="product_id"]'),
						$product_id_el_val = $t.data('product_id') || ($product_id_el.length ? $product_id_el.val() : $add_to_cart_el.val());
					}

					var prod_id = (typeof $product_id_el_val == 'undefined') ? $t.data('product_id') : $product_id_el_val;

					add_to_cart_info = $cart_form.serializefiles();

					add_to_cart_info.append('context', 'frontend');
					add_to_cart_info.append('action', 'yith_ywraq_action');
					add_to_cart_info.append('ywraq_action', 'add_item');
					add_to_cart_info.append('product_id', $t.data('product_id'));
					add_to_cart_info.append('wp_nonce', $t.data('wp_nonce'));
					add_to_cart_info.append('yith-add-to-cart', $t.data('product_id'));
					var quantity = $t_wrap.find('input.qty').val();

					if (quantity > 0) {
						add_to_cart_info.append('quantity', quantity);
					}

					//compatibility with Woocommerce Product Table by Barn2media
					if ($('.wc-product-table-wrapper').length > 0) {
						var quantity = $t.parents('.product-row').find('.cart input.qty').val();
						if (quantity > 0) {
							add_to_cart_info.append('quantity', quantity);
						}

					}

					//compatibility with YITH Quick Order Form
					if ($t.closest('.yith_wc_qof_button_and_price').length > 0) {
						var qof_wrap = $t.closest('.yith_wc_qof_button_and_price'),
						qof_quantity = qof_wrap.find('.YITH_WC_QOF_Quantity_Cart').val();
						add_to_cart_info.append('quantity', qof_quantity);
					}

					// compatibility with color and label
					var wcclForm = $t.closest('li.product').find('.variations_form.in_loop'),
					varID_wccl = wcclForm.length ? wcclForm.data('active_variation') : false;
					if (varID_wccl) {
						add_to_cart_info.append('variation_id', varID_wccl);
						// get select value
						wcclForm.find('select').each(function() {
							add_to_cart_info.append(this.name, this.value);
						});
					}

					$(document).trigger('yith_ywraq_action_before', $t, add_to_cart_info);

					// Easy order page integration ________
					if (typeof yith_wceop_params !== 'undefined' && typeof yith_wceop_params.do_submit !== 'undefined') {
						if (!yith_wceop_params.do_submit) {
							return false;
						}

						if (typeof yith_wceop_params !== 'undefined' && typeof yith_wceop_params.ywraq_quantity !== 'undefined' && yith_wceop_params.ywraq_quantity > 0) {
							add_to_cart_info.append('quantity', yith_wceop_params.ywraq_quantity);
						}

						var variationID = $t.data('variation_id');

						if (variationID) {
							add_to_cart_info.append('variation_id', variationID);
						}

						if ($t.data('variation_args')) {
							var variationArgs = jQuery.parseJSON(decodeURIComponent($t.data('variation_args')));
							for (var key in variationArgs) {
								add_to_cart_info.append(key, variationArgs[key]);
							}
						}
					}

					// Product addons integration ________
					if (typeof yith_wapo_general !== 'undefined') {

						if (!yith_wapo_general.do_submit) {
							return false;
						}

					}

					if (typeof ywcnp_raq !== 'undefined') {
						if (!ywcnp_raq.do_submit) {
							return false;
						}
					}

					xhr = $.ajax({
						type: 'POST',
						url: ywraq_frontend.ajaxurl.toString().replace('%%endpoint%%', 'yith_ywraq_action'),
						dataType: 'json',
						data: add_to_cart_info,
						contentType: false,
						processData: false,
						beforeSend: function() {
							$t.after(' <img src="' + ajax_loader + '" class="ywraq-loader" >');
						},
						complete: function() {
							$t.next().remove();
						},

						success: function(response) {
							if (response.result == 'true' || response.result == 'exists') {
								
								var variationID = $t.data('variation_id');

								if (ywraq_frontend.go_to_the_list == 'yes') {
									window.location.href = response.rqa_url;
								} else { console.log(variationID);
									$t.parent().siblings('.yith_ywraq_add_item_response-' + variationID).hide().addClass('hide').html('');
									$t.parent().siblings('.yith_ywraq_add_item_product-response-' + variationID).show().removeClass('hide').html(response.message);
									$t.parent().siblings('.yith_ywraq_add_item_browse-list-' + variationID).show().removeClass('hide');
									$t.parent().hide().removeClass('show').addClass('addedd');
									$t.closest('.pvt-yith-quote-support').find('input.qty').hide()
									$('.add-to-quote-' + prod_id).attr('data-variation', response.variations);

									if ($widget.length) {
										$widget.ywraq_refresh_widget();
										$widget = $(document).find('.widget_ywraq_list_quote, .widget_ywraq_mini_list_quote');
									}

									ywraq_refresh_number_items();
								}

								$(document).trigger('yith_wwraq_added_successfully', [response, prod_id]);

							} else if (response.result == 'false') {
								$('.yith_ywraq_add_item_response-' + prod_id).show().removeClass('hide').html(response.message);

								$(document).trigger('yith_wwraq_error_while_adding');
							}
							xhr = false;
						},
					});

				});
				
				function ywraq_refresh_number_items() {
					var $number_items = $(document).find('.ywraq_number_items');
					$number_items.each(function() {
					  var $t = $(this),
						  show_url = $t.data('show_url'),
						  item_name = $t.data('item_name'),
						  item_plural_name = $t.data('item_plural_name');

					  $.ajax({
						type: 'POST',
						url: ywraq_frontend.ajaxurl.toString().replace('%%endpoint%%', 'yith_ywraq_action'),
						data: 'ywraq_action=refresh_number_items&action=yith_ywraq_action&context=frontend&item_name=' + item_name + '&item_plural_name=' + item_plural_name + '&show_url=' + show_url,
						success: function(response) {
						  $t.replaceWith(response);
						  $(document).trigger('ywraq_number_items_refreshed');
						},
					  });

					});
				  };


			}(jQuery))

		</script>
	<?php

}, 99 );

// Remove the default YITH Quote button when variable product

add_action('template_redirect', function(){

	if( is_woocommerce() ){
		$adding_to_quote = wc_get_product( get_the_ID() );

		if( $adding_to_quote->is_type( 'variable' ) ){
 			//remove_action( 'woocommerce_single_product_summary', array( YITH_YWRAQ_Frontend::get_instance(), 'add_button_single_page' ), 35 );
			remove_action( 'woocommerce_before_single_product', array( YITH_YWRAQ_Frontend::get_instance(), 'show_button_single_page' ), 0 );
		}
	}

});
```
