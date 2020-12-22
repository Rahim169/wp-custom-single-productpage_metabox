# wp-custom-single-productpage_metabox
input product card  Item  data,Product get  item data show card, Product show Order,artwordtype attach file




<?php
add_action( 'woocommerce_single_variation', 'display_additional_product_fields', 9 );
function display_additional_product_fields(){
	$upload_path = wp_upload_dir();
	$atwork_url = $upload_path["baseurl"].'/artwork/';
	$plugin_dir_url = plugin_dir_url( __FILE__ );


	global $post;
	$terms = get_the_terms( $post->ID, 'product_cat' );
	$produtct_cat_ids[] = '';
	foreach ($terms as $term) {
	    $produtct_cat_ids[] = $term->term_id;
	}

	$custom_font_familay = explode(',', '72,36,38,87');
	$font_text_line = explode(',', '36,38,87');
?>

<script src="<?php echo plugin_dir_url( __DIR__ ); ?>assets/jquery.fontselect.js"></script>
<link rel="stylesheet" type="text/css" href="<?php echo plugin_dir_url( __DIR__ ); ?>assets/fontselect-alternate.css">



<style>
	.artwork_input_field{
		height: 100px;
    	margin-top: 10px;
	}
	.artwork_attach_file {
		border: none;
		height: 100px; 
	}
	#artwork_attach_file{
		padding: 35px;
    	padding-left: 33%;
    	background: #d5d5d5;
	}
    #productPriceTable td{
	    padding: 0.75rem;
	    border-top: 1px solid #e9ecef;
	    font-family: Poppins, Arial, Helvetica, sans-serif;
	    color: #2d2a2a;
	    font-weight: 600;
	    line-height: 1.2;
    }
    #ppom-price-container{
    	display: none;
    }

    #front_text{
      width:90%;
      border-radius: 5px;
    }
    .fronttext{
	    width: 104%;
	    margin-top: 56px;
	    margin-left: -12px;
	    margin-bottom: -35px;
    }
    .add_font_text{
	    float: right;
	    width: 10%;
	    padding-bottom:0px;
	    padding-top:0px;
	    background: #479de6;
	    color: #fff;
	    font-size: 26px !important;
    }
    #fronttext a{
    	background: #479de6;
    }
    .remove_font_text{
	    float: right;
	    width: 10%;
	    padding-bottom:0px;
	    padding-top:0px;
	    background: red;
	    color: #fff;
	    font-size: 26px !important;
    }
    .fa{
	    height: 40px;
	    padding-top: 10px;
    }
    .modal-body{
    	font-size: 20px;
    	color: #3403f9;
        font-weight: 600;
    }
    .btn-secondary{
        background-color: #e0b252;
        border-radius: 10px;
    }
    .modal-header{
       background: #e0b252;
    }
    .modal-header h5{
    	color: #000;
	    
	    font-weight: 700;
    }
    .modal-dialog-centered{
    	margin-top: 20% !important;
    }

  
</style>

	<div class="modal fade" id="alertmodal" tabindex="-1" role="dialog" aria-labelledby="exampleModalCenterTitle" aria-hidden="true">
	  <div class="modal-dialog modal-dialog-centered" role="document">
	    <div class="modal-content">
	      <div class="modal-header">
	        <h2 class="modal-title" id="exampleModalLongTitle">WARNING</h2>
	        <button type="button" class="close" data-dismiss="modal" aria-label="Close">
	          <span aria-hidden="true">&times;</span>
	        </button>
	      </div>
	      <div class="modal-body">
	       
	      </div>
	      <div class="modal-footer">
	        <button type="button" class="btn btn-secondary" data-dismiss="modal">Close</button>
	      </div>
	    </div>
	  </div>
	</div>



	<script>
		jQuery(document).ready(function(){

			/*
			// For Wristband category only *******start
			 // font_textand_fontfamily_display_field with use different category Id
			font_textand_family_display_field = "<?php if (array_intersect($font_textand_family_display_field_supported_cat_for_wristband, $produtct_cat_ids)) { echo 'show'; } ?>"
	       	//for font select picker//
			jQuery('.back_message_free__').after('<div class="container mt-5 mb-5"><h5>Font Select picker</h5><input id="fonts" type="text"></div>');
			jQuery('#fonts').fontselect();
			font_family = jQuery('#fonts').val();
			*/


			custom_font_familay = "<?php if (array_intersect($custom_font_familay, $produtct_cat_ids)) { echo 'show'; } ?>";
			font_text_line = "<?php if (array_intersect($font_text_line, $produtct_cat_ids)) { echo 'show'; } ?>";

			if (custom_font_familay == 'show') {
				jQuery('.artwork_type__please_upload_high_quality_picture_').before('<div class="container mt-5 mb-5"><h5>Font Select picker</h5><input id="fonts" name="custom-font" type="text"></div>');
				jQuery('#fonts').fontselect();
				font_family = jQuery('#fonts').val();
			}

			if (font_text_line == 'show') {
				jQuery('.font-select').after('<div id="fronttext" class="fronttext"><label for="front_text">Front Text Line 1</label><input type="text" name="front_text[]" id="front_text"><a data-id="fronttext" class="add_font_text">+</a></div>');
				serial = 1;
				jQuery(".add_font_text").click(function(){
					serial++;
					fonttextlength = jQuery('.fronttext').length;
					if (fonttextlength <= 3) {
			        	jQuery('.fronttext:last').after('<div id="fronttext'+serial+'" class="fronttext"><label for="front_text">Front Text Line '+serial+'</label><input type="text" name="front_text[]" id="front_text"><button data-id="fronttext'+serial+'" class="remove_font_text"><i class="fa fa-minus"></i></button></div>');
			        }else{
			        	jQuery('#alertmodal .modal-body').html('We only accept up to 4 Front Text.');
			        	jQuery('#alertmodal').modal('show');		        	
				    }

			        jQuery(document).on('click', '.remove_font_text', function () {
			        	removeid = jQuery(this).attr('data-id');
					    jQuery('#'+removeid).remove();
					});
				});
			}



	            jQuery(".woodmart-swatch").click(function(){
	                setTimeout(function(){
	                    pricing_table_show_hide();
	                }, 500);
	            });

	            function pricing_table_show_hide(){
	                data_value = jQuery('.active-swatch').attr('data-value');
	                jQuery('.wad-qty-pricing-table').hide();
	                jQuery('[variation-name='+data_value+']').show();
	            }
        
			jQuery('#ppom-price-container').after('<div style="display: none;" id="productPriceTable"><table><tr class="optional_price_tr" style="display: none; background-color: rgba(0, 0, 0, 0.05);"><td>Option Total</td><td class="total_optional_price"></td></tr><tr><td>Product Price <span class="product_base_price_qty"></span></td><td class="product_base_price_qty_total"></td></tr><tr style="background-color: rgba(0, 0, 0, 0.05);"><td>Total</td><td class="total_product_price"></td></tr></table></div>');
			
			jQuery('select').on('change', function () {
				 setTimeout(function(){
				 	price_setup_in_single_page();
				 }, 500);
			});
			
			jQuery(".plus, .minus, .woodmart-swatch").click(function(){
				setTimeout(function(){
					price_setup_in_single_page();
				}, 500);
			});

			function price_setup_in_single_page(){
				jQuery('#productPriceTable').show();
				if (jQuery('.summary-inner .price ins .woocommerce-Price-amount bdi').length > 0) {
				   	amount_price_ele = jQuery('.summary-inner .price ins .woocommerce-Price-amount bdi').html();
					amount_price = amount_price_ele.replace('<span class="woocommerce-Price-currencySymbol">৳&nbsp;</span>','');
				}else{
				    amount_price_ele = jQuery('.summary-inner .woocommerce-Price-amount bdi').html();
					amount_price = amount_price_ele.replace('<span class="woocommerce-Price-currencySymbol">৳&nbsp;</span>','');
				}
				quantity = jQuery('.quantity .qty').val();
				total_ammount = quantity*parseInt(amount_price);
				jQuery('.product_base_price_qty').html(' (৳'+amount_price+' X '+quantity+')');
				jQuery('.product_base_price_qty_total').html('৳'+total_ammount.toFixed(2));        

				if (jQuery(".ppom-option-total-price #ppom-price-cloner .ppom-price").length > 0) {
		    		totalOptionalPrice = jQuery(".ppom-option-total-price #ppom-price-cloner .ppom-price").html();
					jQuery('.total_optional_price').html('৳'+totalOptionalPrice);
					jQuery('.optional_price_tr').show();
		    	}else{
		    		jQuery('.optional_price_tr').hide();
		    		totalOptionalPrice = 0;
		    	}
				TotalPrice = parseInt(total_ammount)+parseInt(totalOptionalPrice);
				jQuery('.total_product_price').html('৳'+TotalPrice.toFixed(2));
			}


			jQuery('#artwork_type__please_upload_high_quality_picture_').change(function(){
				artwork_type = jQuery(this).val();
				if (artwork_type == 'Upload My Artwork') {
					jQuery('.artwork_place').show();
				}else if(artwork_type == 'None'){
					jQuery('.artwork_place').hide();
					jQuery('.artwork_img_preview').remove();
					jQuery('.attach_file').val('');
				}
			});
		jQuery('#artwork_type__please_upload_high_quality_picture_').after('<div class="artwork_place" style="display: none;"><d iv class="artwork_input_field"><!--label for="artwork_attach_file">Upload Artwork</label--> <input type="file" accept=".jpg, .jpeg, .png" name="artwork_attach_file" class="artwork_attach_file form-control" id="artwork_attach_file"><input type="hidden" name="attach_file" class="attach_file"></div><div class="artwork_previw_img_place"></div></div>');

	    jQuery('.artwork_attach_file').change(function(){
		    formdata = new FormData();
		    if(jQuery(this).prop('files').length > 0){
		        file = jQuery(this).prop('files')[0];
		        formdata.append("artwork_attach_file", file);
		    }

		    jQuery.ajax({
			    url: '<?php echo $plugin_dir_url ?>upload.php',
			    type: "POST",
			    data: formdata,
			    processData: false,
			    contentType: false,
			    success: function (result) {
					var data_result = result.split(',');
					if (data_result[0] == 'success') {
						img_url = '<?php echo $atwork_url ?>'+data_result[1];
						jQuery('.artwork_img_preview').remove();
						//jQuery('.attach_file').val('<?php echo $atwork_url ?>'+data_result[1]);
						img_preview = '<img class="artwork_img_preview" src="'+img_url+'" style="height: 80px; margin-top:10px;">';
						jQuery('.attach_file').val(img_url);
						jQuery('.artwork_previw_img_place').html(img_preview);
					}else{
						alert(result);
					}
			    }
			});		    
		});
	});
	</script>
</div>
<?php
}

//input product card  Item  data
function plugin_republic_add_cart_item_data( $cart_item_data, $product_id, $variation_id ) {
 	if (isset($_POST["attach_file"]) && !empty($_POST["attach_file"])){
		$atwork_file_url = $_POST["attach_file"];
		$cart_item_data['attach_file'] = sanitize_text_field($atwork_file_url);
	} 
	 if( isset( $_POST['custom-font'] ) ) {
	 	$custom_font_familay = $_POST["custom-font"];
      	$cart_item_data['custom-font'] = sanitize_text_field($custom_font_familay);
    }
     if(isset( $_POST['front_text'])) {
	 	$font_text_line = implode(',', $_POST["front_text"]);
      	//$cart_item_data['front_text'] = sanitize_text_field($font_text_line);
      	$cart_item_data['front_text'] = $font_text_line;
    }
	return $cart_item_data;
}
add_filter( 'woocommerce_add_cart_item_data', 'plugin_republic_add_cart_item_data', 10, 3 );


//Product get  item data show card
function plugin_republic_get_item_data( $item_data, $cart_item_data ) {
	if (isset($cart_item_data["attach_file"]) && !empty($cart_item_data["attach_file"])){
		$item_data[] = array('key' => 'Artwork File- <img class="artwork_img_preview" src="'.$cart_item_data['attach_file'].'" style="height: 50px">');
	}
	 if(isset( $cart_item_data['custom-font'])) {
 	   $item_data[] = array('key' => __( 'Font Family'), 'value' => $cart_item_data['custom-font'] );
    }
     if( isset( $cart_item_data['front_text'] ) ) {
     	$font_text_lines = array();
     	$serial_font_text_line = 0;
     	foreach (explode(',', $cart_item_data['front_text']) as $value) {
     		++$serial_font_text_line;
     		$font_text_lines[] = 'Front Text Line '.$serial_font_text_line.' : '.$value.'<br>';
     	}

 	   $item_data[] = array('key' => __( 'Front Text Line'), 'value' => implode('', $font_text_lines));
    }	
 	return $item_data;
}
add_filter( 'woocommerce_get_item_data', 'plugin_republic_get_item_data', 10, 2 );
	




// Product show Order
function plugin_republic_checkout_create_order_line_item( $item, $cart_item_key, $values, $order ) {
	if(isset($values['attach_file']) && !empty($values['attach_file'])){
		$item->add_meta_data(__( 'Artwork File: '), '<img class="artwork_img_preview" src="'.$values['attach_file'].'" style="height: 50px">', true);
	}
	if( isset($values['custom-font'])) {
    $item->add_meta_data(  __('Font Family'), $values['custom-font'],true);
  }

  if( isset( $values['front_text'] ) ) {
     	$font_text_lines = array();
     	$serial_font_text_line = 0;
     	$font_text_values = explode(',', $values['front_text']);
     	foreach ($font_text_values as $value) {
     		++$serial_font_text_line;
     		$font_text_lines[] = 'Front Text Line '.$serial_font_text_line.' : '.$value.'<br>';
     	}

 	  $item->add_meta_data(  __('Front Text Line'), implode('', $font_text_lines),true);
    }	
 	return $item_data;
}
add_action( 'woocommerce_checkout_create_order_line_item', 'plugin_republic_checkout_create_order_line_item', 10, 4 );
