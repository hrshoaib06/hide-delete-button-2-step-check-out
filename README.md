# hide-delete-button-2-step-check-out
Hide Delete button on 2-step cart checkout for order guide items - Servicenow

UI Macro - sc_cart_view_column_item_edit-custom_ver
***************************************************

<?xml version="1.0" encoding="utf-8" ?>
    <j:jelly trim="false" xmlns:j="jelly:core" xmlns:g="glide" xmlns:j2="null" xmlns:g2="null">
	<g:include_script src="jquery.jsdbx"/>
    <div class="sc_cv_edit_items_buttons">
		<script>
			var itemDelUrl = null;
			if (typeof showDeleteConfirmDialog == "undefined")
				function showDeleteConfirmDialog(url,showDialog) {
					if(!showDialog){
						deleteItem(url);
						return;
					}
        			itemDelUrl = url;	
        			var dialogClass = window.GlideModal ? GlideModal : GlideDialogWindow;
        			var dialog = new dialogClass('glide_confirm_standard');
        			dialog.setTitle(new GwtMessage().getMessage('Confirmation'));
        			dialog.setPreference('warning', true);
        			dialog.setPreference('title', new GwtMessage().getMessage("delete_order_guide_entries"));
        			dialog.setPreference('onPromptComplete', deleteItem);
        			dialog.render();
                }

			if (typeof deleteItem == "undefined")
				function deleteItem(url) {
					if(!url)
						url = itemDelUrl;
					document.location.href = url + "$[AMP]url=" + encodeURIComponent(document.location.href);
				}
		</script>

		<g:evaluate var="jvar_verify_order_guide_delete" jelly="true">
                if(${jvar_order_guide_delete}>1)
					true;
				else 
					false;
             </g:evaluate>   

		<j:if test="${jvar_sc_delete_item_button == 'true'}">
            <g:evaluate var="jvar_cart_remove_url" jelly="true">
                var jvar_cart_remove_url = GlideappCatalogURLGenerator.getCartRemoveURL(jelly.sysparm_catalog, jelly.sysparm_catalog_view, jelly.jvar_cart_item.getGr().getUniqueValue(), gs.getSessionToken());
                jvar_cart_remove_url;
            </g:evaluate>

            <g:evaluate jelly="true" expression="var sc_delete_item_label=gs.getMessage(jelly.jvar_sc_delete_item_label);" />

            <g:sc_button id="sc_cv_edit_items_delete_button_id" 
            			img="" 
            			classes="request_catalog_button" 
            			onclick="showDeleteConfirmDialog('${jvar_cart_remove_url}',${jvar_verify_order_guide_delete}); return false;" 
            			label="${sc_delete_item_label}" 
						title="${sc_delete_item_label} ${jvar_cat_item}" 
            			isDanger="true"/>

		</j:if>
		 <script>
			 $('button[aria-label="Delete Acrobat"]').remove();
		</script>
        <j:if test="${jvar_sc_edit_item_button == 'true'}">
            <g:evaluate var="jvar_edit_cart_action" jelly="true">
                var jvar_edit_cart_url = GlideappCatalogURLGenerator.getFullItemEditURL(jelly.jvar_cat_item.getID(), jelly.sysparm_catalog, jelly.sysparm_catalog_view, jelly.jvar_cart_item.getGr().getUniqueValue(), jelly.sysparm_view, jelly.sysparm_processing_hint);
                var jvar_edit_cart_action = "location.assign('" + jvar_edit_cart_url + "'); return false;";        
                jvar_edit_cart_action;
            </g:evaluate>
            <g:evaluate jelly="true" expression="var sc_edit_item_label=gs.getMessage(jelly.jvar_sc_edit_item_label);" />
            <g:sc_button id="sc_cv_edit_items_edit_button_id" 
            			img="" 
            			classes="request_catalog_button" 
            			onclick="${jvar_edit_cart_action}" 
            			label="${sc_edit_item_label}" 
            			title="${sc_edit_item_label} ${jvar_cat_item}"/>
        </j:if>
				 
    </div>	
</j:jelly>
