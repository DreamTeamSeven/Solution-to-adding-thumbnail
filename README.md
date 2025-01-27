# Solution for inserting thumbnail correctly into the Product Page
## Steps
1. First I viewed the source code from the html doc with the thumb nail not working. I copied the code to [notWorkingHtmlPlugin.html](notWorkingHtmlPlugin.html).
2. Then I viewed the html with no plugin active and saved it to [workingHtmlNoPlugin.html](workingHtmlNoPlugin.html).
3. The next step was comparing the working html(no plugin) to the html out code was producing:
### Html (no plugin/working correctly)
```html
<div class="nv-single-product-top">
    <div class="woocommerce-product-gallery--with-images images woocommerce-product-gallery woocommerce-product-gallery--columns-4"
        data-columns="4" style="opacity: 0; transition: opacity .25s ease-in-out;">
        <!--Below here -->
        <div class="woocommerce-product-gallery__wrapper">
            <div data-thumb="http://10.0.0.219/wp-content/uploads/2025/01/chair-100x100.webp" data-thumb-alt="fgdfg"
                data-thumb-srcset="http://10.0.0.219/wp-content/uploads/2025/01/chair-100x100.webp 100w, http://10.0.0.219/wp-content/uploads/2025/01/chair-150x150.webp 150w, http://10.0.0.219/wp-content/uploads/2025/01/chair.webp 275w"
                data-thumb-sizes="(max-width: 100px) 100vw, 100px" class="woocommerce-product-gallery__image"><a
                    href="http://10.0.0.219/wp-content/uploads/2025/01/chair.webp"><img width="275" height="271"
                        src="http://10.0.0.219/wp-content/uploads/2025/01/chair.webp" class="wp-post-image" alt="fgdfg"
                        data-caption="dfg" data-src="http://10.0.0.219/wp-content/uploads/2025/01/chair.webp"
                        data-large_image="http://10.0.0.219/wp-content/uploads/2025/01/chair.webp"
                        data-large_image_width="275" data-large_image_height="271" decoding="async" fetchpriority="high"
                        srcset="http://10.0.0.219/wp-content/uploads/2025/01/chair.webp 275w, http://10.0.0.219/wp-content/uploads/2025/01/chair-100x100.webp 100w"
                        sizes="(max-width: 275px) 100vw, 275px" /></a></div>
                        
```
### Html (plugin not working correctly)
```html
<div class="nv-single-product-top">
    <div class="woocommerce-product-gallery--with-images images woocommerce-product-gallery woocommerce-product-gallery--columns-4"
        data-columns="4" style="opacity: 0; transition: opacity .25s ease-in-out;">
        <!--Below here -->
        <div class="woocommerce-product-gallery__wrapper">
            <div class="polymuse-model-viewer woocommerce-product-gallery__image" data-gallery-item="3d-model">
                <model-viewer
                    src="https://yiteg94znhby2sle.public.blob.vercel-storage.com/chair-RqAX9DTtK4mSi8e7VZuOllJh0b02NG.glb"
                    alt="3D model of Tester" auto-rotate camera-controls
                    style="width: 100%; height: 100%;"></model-viewer></div>
            <li><img src="http://10.0.0.219/wp-content/plugins/polymuse-woocommerce/3d-model-thumbnail.png"
                    alt="3D Model Thumbnail" class="model-thumbnail" data-gallery-item="3d-model" /></li>
        <!--Above here -->
            <div data-thumb="http://10.0.0.219/wp-content/uploads/2025/01/chair-100x100.webp" data-thumb-alt="fgdfg"
                data-thumb-srcset="http://10.0.0.219/wp-content/uploads/2025/01/chair-100x100.webp 100w, http://10.0.0.219/wp-content/uploads/2025/01/chair-150x150.webp 150w, http://10.0.0.219/wp-content/uploads/2025/01/chair.webp 275w"
                data-thumb-sizes="(max-width: 100px) 100vw, 100px" class="woocommerce-product-gallery__image"><a
                    href="http://10.0.0.219/wp-content/uploads/2025/01/chair.webp"><img width="275" height="271"
                        src="http://10.0.0.219/wp-content/uploads/2025/01/chair.webp" class="wp-post-image" alt="fgdfg"
                        data-caption="dfg" data-src="http://10.0.0.219/wp-content/uploads/2025/01/chair.webp"
                        data-large_image="http://10.0.0.219/wp-content/uploads/2025/01/chair.webp"
                        data-large_image_width="275" data-large_image_height="271" decoding="async" fetchpriority="high"
                        srcset="http://10.0.0.219/wp-content/uploads/2025/01/chair.webp 275w, http://10.0.0.219/wp-content/uploads/2025/01/chair-100x100.webp 100w"
                        sizes="(max-width: 275px) 100vw, 275px" /></a></div>
```
As you can see the code is structured very differently. 
4. Change how we are structuring the html being inserted into the product page.
```php
$model_viewer = '<div data-thumb="' . esc_url($model_thumbnail_url) . '" ';
$model_viewer .= 'data-thumb-alt="3D Model" ';
$model_viewer .= 'data-thumb-srcset="' . esc_url($model_thumbnail_url) . ' 100w" ';
$model_viewer .= 'data-thumb-sizes="(max-width: 100px) 100vw, 100px" ';
$model_viewer .= 'class="woocommerce-product-gallery__image polymuse-model-viewer">';
$model_viewer .= '<model-viewer src="' . esc_url($model_url) . '" alt="3D model of ' . esc_attr($product->get_name()) . '" auto-rotate camera-controls ar style="width: 100%; height: 100%;"></model-viewer>';
$model_viewer .= '</div>';
```
>NOTE
>In the above step I left out the parent a element to allow for proper camera control.
### Final outputted Html(plugin working correctly)
```html
<div class="nv-single-product-top">
    <div class="woocommerce-product-gallery--with-images images woocommerce-product-gallery woocommerce-product-gallery--columns-4"
        data-columns="4" style="opacity: 0; transition: opacity .25s ease-in-out;">
        <!--Below here -->
        <div class="woocommerce-product-gallery__wrapper">
            <div data-thumb="http://10.0.0.219/wp-content/plugins/test-add-to-dom-plugin/3d-model-thumbnail.png"
                data-thumb-alt="3D Model"
                data-thumb-srcset="http://10.0.0.219/wp-content/plugins/test-add-to-dom-plugin/3d-model-thumbnail.png 100w"
                data-thumb-sizes="(max-width: 100px) 100vw, 100px"
                class="polymuse-model-viewer woocommerce-product-gallery__image"><model-viewer
                    src="https://yiteg94znhby2sle.public.blob.vercel-storage.com/chair-RqAX9DTtK4mSi8e7VZuOllJh0b02NG.glb"
                    alt="3D model of Tester" auto-rotate camera-controls ar
                    style="width: 100%; height: 100%;"></model-viewer></div>
``` 
## Differences in code
You can check out the differences in the code in: [codeBlocks.jpg](codeBlocks.jpg)