##1.头条面试算法：已知出栈序列求入栈序列
函数作用：已知出栈序列求入栈序列
函数参数：两个栈  一个数组   一个小标
函数返回值:打印出所有可能的入栈序列
注：开始一看就觉的很不好做，好像和平时的思维方向是相反的，但是仔细一想已知出栈求入栈序列和已知入栈序列求出栈序列好像又是一道题，这是因为 A序列进栈–》B序列 那莫B序列进栈必然可以得到A序列
思想：就是说以1,2,3,4,进栈为例，对于4来说就是两种情况要么最先被入栈要么最后被入栈，（1,2,3）可以递归求解，，代码如下
```
public class AllPopSeq {
	  public static void main(String[] args){
		    int j=1;
	        int[] sequence = new int[4];
	 
	        /*按顺序输入入栈元素的编号*/
	        for (int i=0; i<4; i++){
	            sequence[i] = j++;
	        }
	        /*没有出栈的压栈到stack栈中，输出的时候逆序输出*/
	        Stack<Integer> stack = new Stack<Integer>();
	        /*出栈的元素压栈到queue栈中，输出的时候正序输出*/
	        Stack<Integer> queue = new Stack<Integer>();
	 
	        /*
	        *1、初始状态有stack、queue两个栈，可以进行两个操作，一个是sequence中的元素入栈stack，对应in函数，一个是
	        *   出栈操作，stack中的元素出栈（stack中元素不为空），压到queue栈中，第一个操作执行完了之后执行第二个。
	        *2、每次执行读入元素或者元素出栈之后都可以重复刚才所说操作，继续读入数据，然后出栈。
	        *3、当读取到sequence最后一个元素的时候，先正序输出queue中的元素，在逆序输出stack中的元素，然后回退。
	        *4、回退的时候要把原来的操作撤销，比如读入数据的stack.push(sequence[nextPosition]);in函数执行完了之后要撤销就是
	        *   stack.pop();对应得in函数也是：queue.push(stack.pop())对应：if (!queue.isEmpty()) stack.push(queue.pop());
	        */
	        next(stack,sequence,0,queue);
	    }
	 
	    public static void in(Stack<Integer> stack,int[] sequence,int nextPosition,Stack<Integer> queue){
	        if (nextPosition == sequence.length) {
	            String result = "";
	            for(int i=0; i< queue.size(); i++){
	                result+=queue.elementAt(i)+" ";
	            }
	            for (int i=stack.size()-1; i>=0; i--){
	                result+=stack.elementAt(i)+" ";
	            }
	            System.out.println(result.substring(0,result.length()-1));
	            return;
	        }
	        next(stack, sequence, nextPosition, queue);
	    }
	 
	    public static void next(Stack<Integer> stack,int[] sequence,int nextPosition, Stack<Integer> queue){
	        stack.push(sequence[nextPosition]);
	        in(stack, sequence, nextPosition+1, queue);
	        stack.pop();
	 
	        if (!stack.isEmpty()) {
	            queue.push(stack.pop());
	            next(stack,sequence,nextPosition,queue);
	            if (!queue.isEmpty()) stack.push(queue.pop());
	        }
	    }
}
```
## 2.头条算法：  字符串问题：
要求：1.三个同样的字母连在一起一定是个错误，去掉一个
      2.两个成一个队形出现的字母去掉第二个的字母其中一个  比如AABB去掉B

```
public class AllPopSeq {
	public static void main(String[] args) {
		Scanner in = new Scanner(System.in);
		System.out.println("输入：");
		int num=in.nextInt();
		String str[]=new String[num];
		for(int lp=0;lp<num;lp++) {
			str[lp]=in.next();
		}
		System.out.println("输出：");
		for(String s:str) {
			System.out.println(AllPopSeq.filterCode3(AllPopSeq.filterCode2(s)));
		}
	}
 
	public static String filterCode2(String str) {
		List<String> list = new ArrayList<String>();
		for (int i = 0; i < str.length(); i++) {
			list.add(str.charAt(i) + "");// 这里就是使用字符串存入到list中，
		}
		for (int m = 0; m < list.size() -2; m++) {
			if ((list.get(m).equals(list.get(m + 1))&&(list.get(m + 1)).equals(list.get(m + 2))  )) {
				list.remove(m+1);
				m--;				
			}
		}
		StringBuffer sb=new StringBuffer();
		String str3="";
		for(int j=0;j<list.size();j++){
			str3=sb.append(list.get(j)).toString();
		}
		return str3;
	}
	
	public  static String filterCode3(String str) {
		int sumnumber=0;
		List<String> list = new ArrayList<String>();
		for (int a = 0; a < str.length(); a++) {
			list.add(str.charAt(a) + "");}
		int i=0;
		int []str1=new int [str.length()];
		char[] c = str.toCharArray();//将字符串转化为字符数组，便于遍历每个字符
		str1[0]=1;
		for(int l=1;l<c.length;l++) {
			if(c[l]==c[l-1]) {
				str1[l-1]++;
			}else {
				str1[l]=1;
			}
		}
		str1=AllPopSeq.newarray(str1);
		for(int j=0;j<str1.length-1;j++) {
			if(str1[j]!=0) {
			if(str1[j]==str1[j+1]&&(str1[j]>1)) {
				for(int num=0;num<j+1;num++) {
					sumnumber=sumnumber+str1[num];	
				}
				list.remove(sumnumber);
		    }	
			}
		}
		StringBuffer sb=new StringBuffer();
		String str4="";
		for(int j=0;j<list.size();j++){
			str4=sb.append(list.get(j)).toString();
		}
		return str4;
	}
	
	
	public  static int [] newarray(int oldarr[]) {
		int count=0;
		for(int i=0;i<oldarr.length;i++){
			if(oldarr[i]==0){
				count++;
			}
		}
```
