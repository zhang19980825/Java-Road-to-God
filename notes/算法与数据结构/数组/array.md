## 1.有序数组的合并问题
函数作用：有序数组的合并
函数参数：a，b两个数组
函数返回值：返回合并后的数组
注：这个应该是最简单的数组类的算法，注释就不用了
```
public class ArrayMerging {
	public static void main(String[] args) {
		int a[]= {1,3,5,7,8,9};
		int b[]= {2,4,6,8,10};
		int array[]=ArrayMerging.Arraymerging(a, b);
		System.out.println(Arrays.toString(array));
	}
	//两个有序数组的合并
	public static int [] Arraymerging(int a[],int b[]) {
		 //i,j,k分别用来表示三个数组的下标
		int i=0;
		int j=0;
		int k=0;
		int length1=a.length;
		int length2=b.length;
		int newArray[]=new int[length1+length2];  
		while(i<length1&&j<length2) {
			if(a[i]<b[j]) {
				newArray[k++]=a[i++];
			}
			else if(a[i]==b[j]) {
				newArray[k++]=a[i];
				i++;
				j++;
			}
			else {
				newArray[k++]=b[j++];
			}
		}
		while(i<a.length) {
			newArray[k++]=a[i++];
		}
		while(j<b.length) {
			newArray[k++]=b[j++];
		}
		return newArray;
	}
}

```