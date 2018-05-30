```PHP
    public static function removeXSS($str) {
        $str = str_replace('<!--  -->', '', $str);
        $str = preg_replace('~/\*[ ]+\*/~i', '', $str);
        $str = preg_replace('/\\\0{0,4}4[0-9a-f]/is', '', $str);
        $str = preg_replace('/\\\0{0,4}5[0-9a]/is', '', $str);
        $str = preg_replace('/\\\0{0,4}6[0-9a-f]/is', '', $str);
        $str = preg_replace('/\\\0{0,4}7[0-9a]/is', '', $str);
        $str = preg_replace('/&#x0{0,8}[0-9a-f]{2};/is', '', $str);
        $str = preg_replace('/&#0{0,8}[0-9]{2,3};/is', '', $str);
        $str = preg_replace('/&#0{0,8}[0-9]{2,3};/is', '', $str);

        $str = htmlspecialchars($str);
        //$str = preg_replace('/&lt;/i', '<', $str);
        //$str = preg_replace('/&gt;/i', '>', $str);

        // 非成对标签
        $lone_tags = array("img", "param", "br", "hr");
        foreach ($lone_tags as $key => $val)
        {
            $val = preg_quote($val);
            $str = preg_replace('/&lt;' . $val . '(.*)(\/?)&gt;/isU', '<' . $val . "\\1\\2>", $str);
            $str = self::transCase($str);
            $str = preg_replace_callback('/<' . $val . '(.+?)>/i', create_function('$temp', 'return str_replace("&quot;","\"",$temp[0]);'), $str);
        }
        $str = preg_replace('/&amp;/i', '&', $str);

        // 成对标签
        $double_tags = array("table", "tr", "td", "font", "a", "object", "embed", "p", "strong", "em", "u", "ol", "ul", "li", "div", "tbody", "span", "blockquote", "pre", "b", "font");
        foreach ($double_tags as $key => $val)
        {
            $val = preg_quote($val);
            $str = preg_replace('/&lt;' . $val . '(.*)&gt;/isU', '<' . $val . "\\1>", $str);
            $str = self::transCase($str);
            $str = preg_replace_callback('/<' . $val . '(.+?)>/i', create_function('$temp', 'return str_replace("&quot;","\"",$temp[0]);'), $str);
            $str = preg_replace('/&lt;\/' . $val . '&gt;/is', '</' . $val . ">", $str);
        }
        // 清理js
        $tags = Array(
            'javascript',
            'vbscript',
            'expression',
            'applet',
            'meta',
            'xml',
            'behaviour',
            'blink',
            'link',
            'style',
            'script',
            'embed',
            'object',
            'iframe',
            'frame',
            'frameset',
            'ilayer',
            'layer',
            'bgsound',
            'title',
            'base',
            'font'
        );

        foreach ($tags as $tag)
        {
            $tag = preg_quote($tag);
            $str = preg_replace('/' . $tag . '\(.*\)/isU', '\\1', $str);
            $str = preg_replace('/' . $tag . '\s*:/isU', $tag . '\:', $str);
        }

        $str = preg_replace('/[\s]+on[\w]+[\s]*=/is', '', $str);

        Return $str;
    }

    public static function transCase($str) {
        $str = preg_replace('/(e|ｅ|Ｅ)(x|ｘ|Ｘ)(p|ｐ|Ｐ)(r|ｒ|Ｒ)(e|ｅ|Ｅ)(s|ｓ|Ｓ)(s|ｓ|Ｓ)(i|ｉ|Ｉ)(o|ｏ|Ｏ)(n|ｎ|Ｎ)/is', 'expression', $str);

        Return $str;
    }
```