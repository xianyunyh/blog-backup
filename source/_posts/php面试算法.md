title: php算法
date: 2017-05-03 22:12:07 
tags: [php]

---

### 冒泡排序

	  // 冒泡排序
    function bubble_sort(&$arr){
        for ($i=0,$len=count($arr); $i < $len; $i++) {
            for ($j=1; $j < $len-$i; $j++) {
                if ($arr[$j-1] > $arr[$j]) {
                    $temp = $arr[$j-1];
                    $arr[$j-1] = $arr[$j];
                    $arr[$j] = $temp;
                }
            }
        }
    }
### 快速排序

	function quick_sort($array) {
		if(count($array) <= 1) return $array;
		$key =$array[0];
		$left_arr =array();
		$right_arr =array();
		for ($i=1;$i;$i++){      
		      if ($array[$i] <= $key){
		         $left_arr[] = $array[$i];
		      }else{   
		         $right_arr[] = $array[$i];
		      }
		}
		$left_arr =quick_sort($left_arr);
		$right_arr =quick_sort($right_arr);
		return array_merge($left_arr, array($key), $right_arr);
	}

### 插入排序

		// 区分 哪部分是已经排序好的
	    // 哪部分是没有排序的
	    // 找到其中一个需要排序的元素
	    //这个元素 就是从第二个元素开始，到最后一个元素都是这个需要排序的元素
	    //利用循环就可以标志出来
	    //i循环控制 每次需要插入的元素，一旦需要插入的元素控制好了，
	    //间接已经将数组分成了2部分，下标小于当前的（左边的），是排序好的序列
	    
	function insert_sort($arr) {
	    $len=count($arr);
	    for ($i=1; $i < $len; $i++) {
		        //获得当前需要比较的元素值。
	        $tmp = $arr[$i];
	        //内层循环控制 比较 并 插入
	        for ($j = $i - 1; $j >= 0; $j--) {
	            //$arr[$i];//需要插入的元素; $arr[$j];//需要比较的元素
	            if ($tmp < $arr[$j]) {
	                //发现插入的元素要小，交换位置
	                //将后边的元素与前面的元素互换
	                $arr[$j + 1] = $arr[$j];
	                //将前面的数设置为 当前需要交换的数
	                $arr[$j] = $tmp;
	            } else {
	                //如果碰到不需要移动的元素
	                //由于是已经排序好是数组，则前面的就不需要再次比较了。
	                break;
	            }
	        }
	    }
    //将这个元素 插入到已经排序好的序列内。
    //返回
    return $arr;
}

### 选择排序


	function select_sort($arr) {
	    //实现思路 双重循环完成，外层控制轮数，当前的最小值。内层 控制的比较次数
	    //$i 当前最小值的位置， 需要参与比较的元素
	    $len = count($arr);
	    for ($i = 0; $i < $len - 1; $i++) {
	        //先假设最小的值的位置
	        $p = $i;
	        //$j 当前都需要和哪些元素比较，$i 后边的。
	        for ($j = $i + 1; $j < $len; $j++) {
	            //$arr[$p] 是 当前已知的最小值
	            if ($arr[$p] > $arr[$j]) {
	                //比较，发现更小的,记录下最小值的位置；并且在下次比较时，
	                // 应该采用已知的最小值进行比较。
	                $p = $j;
	            }
	        }
	        //已经确定了当前的最小值的位置，保存到$p中。
	        //如果发现 最小值的位置与当前假设的位置$i不同，则位置互换即可
	        if ($p != $i) {
	            $tmp = $arr[$p];
	            $arr[$p] = $arr[$i];
	            $arr[$i] = $tmp;
	        }
	    }
	    //返回最终结果
	    return $arr;
	}
	$arr = array(88, 1, 2, 5, 4, 3, 66, 0);
	$res = selectSort($arr);
	print_r($res);

### 约瑟夫环问题 ###
一群猴子排成一圈，按1，2，...，n依次编号。然后从第1只开始数，数到第m只,把它踢出圈，从它后面再开始数，再数到第m只，在把它踢出去...，如此不停的进行下去，直到最后只剩下一只猴子为止，那只猴子就叫做大王。要求编程模拟此过程，输入m、n,输出最后那个大王的编号

		// 方案一，使用php来模拟这个过程
    function king($n,$m){
        $mokey = range(1, $n);
        $i = 0;

        while (count($mokey) >1) {
            $i += 1;
            $head = array_shift($mokey);//一个个出列最前面的猴子
            if ($i % $m !=0) {
                #如果不是m的倍数，则把猴子返回尾部，否则就抛掉，也就是出列
                array_push($mokey,$head);
            }

            // 剩下的最后一个就是大王了
            return $mokey[0];
        }
    }
    // 测试
    echo king(10,7);

    // 方案二，使用数学方法解决
    function josephus($n,$m){
        $r = 0;
        for ($i=2; $i <= $m ; $i++) {
            $r = ($r + $m) % $i;
        }

        return $r+1;
    }
### 翻转字符

		   //  考虑中文
	function getRev($str,$encoding='utf-8'){
        $result = '';
        $len = mb_strlen($str);
        for($i=$len-1; $i>=0; $i--){
            $result .= mb_substr($str,$i,1,$encoding);
        }
        return $result;
    }
    $string = 'OK你是正确的Ole';
    echo getRev($string);
	// 不考虑中文
		function getRev($str) {
			$ct = str_len($str);
			$new_str ="";
			for($i=0;$i<$ct;$i++){
				$new_str.=$str[$ct-$i-1];
			}
			return $new_str;
		}