> Convert hex to string and vice versa.
```PHP
function String2Hex($string){
    $hex='';
    for ($i=0; $i < strlen($string); $i++){
        $hex .= dechex(ord($string[$i]));
    }
    return $hex;
}
 
 
function Hex2String($hex){
    $string='';
    for ($i=0; $i < strlen($hex)-1; $i+=2){
        $string .= chr(hexdec($hex[$i].$hex[$i+1]));
    }
    return $string;
}
 
// example:
 
$hex = String2Hex("test sentence...");
// $hex contains 746573742073656e74656e63652e2e2e
 
print Hex2String($hex);
```