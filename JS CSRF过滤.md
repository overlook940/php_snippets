> JS安全过滤类
```PHP
    public static function fliter_script($value) {
        $value = preg_replace("/(javascript:)?on(click|load|key|mouse|error|abort|move|unload|change|dblclick|move|reset|resize|submit)/i","&111n\\2",$value);
        $value = preg_replace("/(.*?)<\/script>/si","",$value);
        $value = preg_replace("/(.*?)<\/iframe>/si","",$value);
        $value = preg_replace ("//iesU", '', $value);
        return $value;
    }
```