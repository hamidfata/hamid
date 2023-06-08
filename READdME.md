
    public function settings_page_callback() {
        if (!current_user_can('manage_options')) {
            return;
        }
        ?>
        <div class="wrap">
            <h1><?php echo esc_html(get_admin_page_title()); ?></h1>
            <form method="post" action="options.php">
                <?php
                settings_fields('city_info_plugin_settings');
                do_settings_sections('city-info-plugin');
                submit_button('Save Settings');
                ?>
            </form>
        </div>
        <?php
    }

    public function city_info_shortcode($atts) {
        $atts    $selected_category = sanitize_text_field($selected_category);

        global $wpdb;
        $table_name = $wpdb->prefix . 'city_info';

        $city_info = $wpdb->get_row($wpdb->prepare("SELECT * FROM $table_name WHERE title = %s AND category = %s", $title, $selected_category));

        if (!$city_info) {
            return 'No information found for the specified city and category.';
        }

        $output = '<h2>' . esc_html($city_info->title) . '</h2>';
        $output .= '<h3>Population: ' . esc_html($city_info->population) . '</h3>';
        $output .= '<h3>Entertainment Centers: ' . esc_html($city_info->entertainment_centers) . '</h3>';
        $output .= '<h3>Hotels: ' . esc_html($city_info->hotels) . '</h3>';
        $output .= '<h3>Universities: ' . esc_html($city_info->universities) . '</h3>';
        $output .= '<h3>Historical Centers: ' . esc_html($city_info->historical_centers) . '</h3>';
        $output .= '<h3>Distance to My Store: ' . esc_html($city_info->distance_to_store) . '</h3>';

        if ($city_info->is_capital) {
            $capital_cities = $wpdb->get_results($wpdb->prepare("SELECT * FROM $table_name WHERE is_capital =



