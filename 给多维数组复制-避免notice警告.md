```php
/**
     * 给多维数组复制,避免notice警告
     * @param $data array
     * @param $key_arr array
     * @param $value mixed
     * @return void
     */
    public static function array_multidimensional_set(&$data, $key_arr, $value)
    {
        $cur_arr = null;
        foreach ($key_arr as $key) {
            if ($cur_arr === null) {
                if (!isset($data[$key])) {
                    $data[$key] = array();
                }
                $cur_arr =& $data[$key];
            } else {
                if (!isset($cur_arr[$key])) {
                    $cur_arr[$key] = array();
                }
                $cur_arr = &$cur_arr[$key];
            }
        }
        $cur_arr = $value;
    }
```
