> 生成随机密码字符串
```PHP
    function randomStr($length = 8, $flag = self::FLAG_NO_NUMERIC) {
        switch ($flag)
        {
            case self::FLAG_NUMERIC:
                $str = '0123456789';
                break;
            case self::FLAG_NO_NUMERIC:
                $str = 'ABCDEFGHIJKLMNOPQRSTUVWXYZ';
                break;
            case self::FLAG_ALPHANUMERIC:
            default:
                $str = 'abcdefghijkmnopqrstuvwxyz0123456789ABCDEFGHIJKLMNOPQRSTUVWXYZ';
                break;
        }

        for ($i = 0, $passwd = ''; $i < $length; $i++)
            $passwd .= Tools::substr($str, mt_rand(0, Tools::strlen($str) - 1), 1);

        return $passwd;
    }

```