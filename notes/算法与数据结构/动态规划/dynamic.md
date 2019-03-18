## 1.两个字符串的最长公共无重复子串
函数作用：求两个字符串的最长公共无重复子串
函数参数：两个字符串
函数返回值:最长不重复公共子串
注：最容易理解的动态规划问题吧，不是很难
```
public class LongestCommonSubstring {
	public static void main(String[] args) {
		String a="helloowoow";
		String b="hllowoowhello";
		String str=LongestCommonSubstring.longComStr(a, b);
		System.out.println(str);
		
	}

	//很明显，动态规划的思想，新建一个数组用来存储字符串a中的i到字符串中的b的j为止最长的公共不重复子串，具体见代码
	public static String  longComStr(String a,String b) {
		if(a.length()==0||a==""||b.length()==0||b=="") {
			return null;
		}
		String str="";//返回的最长子串
		int len1=a.length();
		int len2=b.length();
		int max=0;//最长的公共不重复子串的长度
		//array数组用来存储储字符串a中的i到字符串中的b的j为止最长的公共不重复子串的长度
		int array[][]=new int[len1][len2];
		for(int i=0;i<len1;i++) {
			for(int j=0;j<len2;j++) {
				if(a.charAt(i)==b.charAt(j)) {
					//边界的特殊情况
					if(i==0||j==0) {
						array[i][j]=1;
					}else {
						array[i][j]=array[i-1][j-1]+1;
					}
					if(max<array[i][j]) {
						max=array[i][j];
						//记录当前的最长不重复公共子串的位置
						str=a.substring(i-array[i][j]+1, i+1);//左闭右开的区间
					}
				}
			}
		}
		return str;
	}
}

```