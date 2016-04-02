title: "java各种排序算法实现"
date: 2013-12-10 
tags: [java,算法,排序]
categories: [java,算法]
---


各种排序算法：冒择路(入)兮(稀)快归堆，桶式排序，基数排序
冒泡排序，选择排序，插入排序，稀尔排序，快速排序，归并排序，堆排序，桶式排序，基数排序


<!--more-->


一、冒泡排序(BubbleSort)
1. 基本思想：
　　两两比较待排序数据元素的大小，发现两个数据元素的次序相反时即进行交换，直到没有反序的数据元素为止。
2. 排序过程：
　　设想被排序的数组R［1..N］垂直竖立，将每个数据元素看作有重量的气泡，根据轻气泡不能在重气泡之下的原则，从下往上扫描数组R，凡扫描到违反本原则的轻气泡，就使其向上"漂浮"，如此反复进行，直至最后任何两个气泡都是轻者在上，重者在下为止。

java代码实现：

/**  
    * 冒泡排序：执行完一次内for循环后，最小的一个数放到了数组的最前面（跟那一个排序算法* 不一样）。相邻位置之间交换  
    */    
        
    public class BubbleSort {       
          
        /**    
         * 排序算法的实现，对数组中指定的元素进行排序    
         * @param array 待排序的数组    
         * @param from 从哪里开始排序    
         * @param end 排到哪里    
         * @param c 比较器    
         */      
        public void bubble(Integer[] array, int from, int end) {       
            //需array.length - 1轮比较       
            for (int k = 1; k < end - from + 1; k++) {       
                //每轮循环中从最后一个元素开始向前起泡，直到i=k止，即i等于轮次止       
                for (int i = end - from; i >= k; i--) {       
                    //按照一种规则（后面元素不能小于前面元素）排序       
                    if ((array[i].compareTo(array[i - 1])) < 0) {       
                        //如果后面元素小于了（当然是大于还是小于要看比较器实现了）前面的元素，则前后交换       
                        swap(array, i, i - 1);       
                    }       
                }       
            }       
        }       
            
        /**    
         * 交换数组中的两个元素的位置    
         * @param array 待交换的数组    
         * @param i 第一个元素    
         * @param j 第二个元素    
         */      
        public void swap(Integer[] array, int i, int j) {       
            if (i != j) {//只有不是同一位置时才需交换       
                Integer tmp = array[i];       
                array[i] = array[j];       
                array[j] = tmp;       
            }       
        }       
        
            
        /**     
        * 测试     
        * @param args     
        */      
        public static void main(String[] args) {       
            Integer[] intgArr = { 7, 2, 4, 3, 12, 1, 9, 6, 8, 5, 11, 10 };       
            BubbleSort bubblesort = new BubbleSort();       
            bubblesort.bubble(intgArr,0,intgArr.length-1);    
            for(Integer intObj:intgArr){    
                System.out.print(intObj + " ");    
            }    
        }       
    }      
     

另外一种实现方式：
/** 
    冒泡排序：执行完一次内for循环后，最大的一个数放到了数组的最后面。相邻位置之间交换 
    */  
        public class BubbleSort2{  
        public static void main(String[] args){  
            int[] a = {3,5,9,4,7,8,6,1,2};  
            BubbleSort2 bubble = new BubbleSort2();  
            bubble.bubble(a);  
            for(int num:a){  
                System.out.print(num + " ");  
            }  
        }  
          
        public void bubble(int[] a){  
            for(int i=a.length-1;i>0;i--){  
                for(int j=0;j<i;j++){  
                    if(new Integer(a[j]).compareTo(new Integer(a[j+1]))>0){  
                        swap(a,j,j+1);  
                    }  
                }  
            }  
        }  
          
        public void swap(int[] a,int x,int y){  
            int temp;  
            temp=a[x];  
            a[x]=a[y];  
            a[y]=temp;  
        }  
    }    

二、选择排序
1. 基本思想：
　　每一趟从待排序的数据元素中选出最小（或最大）的一个元素，顺序放在已排好序的数列的最后，直到全部待排序的数据元素排完。
2. 排序过程：


java代码实现：
    /**  
    * 简单选择排序：执行完一次内for循环后最小的一个数放在了数组的最前面。  
    */    
    public class SelectSort {       
          
        /**    
         * 排序算法的实现，对数组中指定的元素进行排序    
         * @param array 待排序的数组    
         * @param from 从哪里开始排序    
         * @param end 排到哪里    
         * @param c 比较器    
         */      
        public void select(Integer[] array) {       
            int minIndex;//最小索引       
            /*    
             * 循环整个数组（其实这里的上界为 array.length - 1 即可，因为当 i= array.length-1    
             * 时，最后一个元素就已是最大的了，如果为array.length时，内层循环将不再循环），每轮假设    
             * 第一个元素为最小元素，如果从第一元素后能选出比第一个元素更小元素，则让让最小元素与第一    
             * 个元素交换     
             */      
            for (int i=0; i<array.length; i++) {       
                minIndex = i;//假设每轮第一个元素为最小元素       
                //从假设的最小元素的下一元素开始循环       
                for (int j=i+1;j<array.length; j++) {       
                    //如果发现有比当前array[smallIndex]更小元素，则记下该元素的索引于smallIndex中       
                    if ((array[j].compareTo(array[minIndex])) < 0) {       
                        minIndex = j;       
                    }       
                }       
          
                //先前只是记录最小元素索引，当最小元素索引确定后，再与每轮的第一个元素交换       
                swap(array, i, minIndex);       
            }       
        }       
            
        public static void swap(Integer[] intgArr,int x,int y){    
            //Integer temp; //这个也行    
            int temp;    
            temp=intgArr[x];    
            intgArr[x]=intgArr[y];    
            intgArr[y]=temp;    
        }    
            
        /**    
         * 测试    
         * @param args    
         */      
        public static void main(String[] args) {       
            Integer[] intgArr = { 5, 9, 1, 4, 2, 6, 3, 8, 0, 7 };       
            SelectSort insertSort = new SelectSort();    
            insertSort.select(intgArr);    
            for(Integer intObj:intgArr){    
                System.out.print(intObj + " ");    
            }      
                
        }       
    }      


三、插入排序(Insertion Sort)
1. 基本思想：
  每次将一个待排序的数据元素，插入到前面已经排好序的数列中的适当位置，使数列依然有序；直到待排序数据元素全部插入完为止。
2. 排序过程：　 

java代码实现：

/**
    * 直接插入排序：
    * 注意所有排序都是从小到大排。
    */ 
     
    public class InsertSort {    
       
        /** 
         * 排序算法的实现，对数组中指定的元素进行排序 
         * @param array 待排序的数组 
         * @param from 从哪里开始排序 
         * @param end 排到哪里 
         * @param c 比较器 
         */   
        public void insert(Integer[] array,int from, int end) {    
       
            /* 
             * 第一层循环：对待插入（排序）的元素进行循环 
             * 从待排序数组断的第二个元素开始循环，到最后一个元素（包括）止 
             */   
            for (int i=from+1; i<=end; i++) {    
                /* 
                 * 第二层循环：对有序数组进行循环，且从有序数组最第一个元素开始向后循环 
                 * 找到第一个大于待插入的元素 
                 * 有序数组初始元素只有一个，且为源数组的第一个元素，一个元素数组总是有序的 
                 */   
                for (int j =0; j < i; j++) {    
                    Integer insertedElem = array[i];//待插入到有序数组的元素   
                    //从有序数组中最一个元素开始查找第一个大于待插入的元素   
                    if ((array[j].compareTo(insertedElem)) >0) {    
                        //找到插入点后，从插入点开始向后所有元素后移一位   
                        move(array, j, i - 1);    
                        //将待排序元素插入到有序数组中   
                        array[j] = insertedElem;    
                        break;    
                    }    
                }    
            } 
                
        }
        
        /** 
         * 数组元素后移 
         * @param array 待移动的数组 
         * @param startIndex 从哪个开始移 
         * @param endIndex 到哪个元素止 
         */   
        public void move(Integer[] array,int startIndex, int endIndex) {    
            for (int i = endIndex; i >= startIndex; i--) {    
                array[i+1] = array[i];    
            }    
        }    
        
      //=======以下是java.util.Arrays的插入排序算法的实现    
        /* 
         * 该算法看起来比较简洁一j点，有点像冒泡算法。 
         * 将数组逻辑上分成前后两个集合，前面的集合是已经排序好序的元素，而后面集合为待排序的
         * 集合，每次内层循从后面集合中拿出一个元素，通过冒泡的形式，从前面集合最后一个元素开 
         * 始往前比较，如果发现前面元素大于后面元素，则交换，否则循环退出 
         *  
         * 总感觉这种算术有点怪怪，既然是插入排序，应该是先找到插入点，而后再将待排序的元素插 
         * 入到的插入点上，那么其他元素就必然向后移，感觉算法与排序名称不匹，但返过来与上面实 
         * 现比，其实是一样的，只是上面先找插入点，待找到后一次性将大的元素向后移，而该算法却 
         * 是走一步看一步，一步一步将待排序元素往前移 
         */   
        /* 
        for (int i = from; i <= end; i++) { 
            for (int j = i; j > from && c.compare(array[j - 1], array[j]) > 0; j--) { 
                swap(array, j, j - 1); 
            } 
        } 
        */   
        
        /** 
         * 测试 
         * @param args 
         */   
        public static void main(String[] args) {    
            Integer[] intgArr = { 5, 9, 1, 4, 2, 6, 3, 8, 0, 7 };    
            InsertSort insertSort = new InsertSort();    
            insertSort.insert(intgArr,0,intgArr.length-1); 
            for(Integer intObj:intgArr){ 
                System.out.print(intObj + " "); 
            } 
        }  
        
        
    }    
       







四、稀尔排序
java代码实现：

public class ShellSort {       
              
            /**    
             * 排序算法的实现，对数组中指定的元素进行排序    
             * @param array 待排序的数组    
             * @param from 从哪里开始排序    
             * @param end 排到哪里    
             * @param c 比较器    
             */      
            public void sort(Integer[] array, int from, int end) {       
                //初始步长，实质为每轮的分组数       
                int step = initialStep(end - from + 1);       
              
                //第一层循环是对排序轮次进行循环。(step + 1) / 2 - 1 为下一轮步长值       
                for (; step >= 1; step = (step + 1) / 2 - 1) {       
                    //对每轮里的每个分组进行循环       
                    for (int groupIndex = 0; groupIndex < step; groupIndex++) {       
              
                        //对每组进行直接插入排序       
                        insertSort(array, groupIndex, step, end);       
                    }       
                }       
            }       
              
            /**    
             * 直接插入排序实现    
             * @param array 待排序数组    
             * @param groupIndex 对每轮的哪一组进行排序    
             * @param step 步长    
             * @param end 整个数组要排哪个元素止    
             * @param c 比较器    
             */      
            public void insertSort(Integer[] array, int groupIndex, int step, int end) {       
                int startIndex = groupIndex;//从哪里开始排序       
                int endIndex = startIndex;//排到哪里       
                /*    
                 * 排到哪里需要计算得到，从开始排序元素开始，以step步长，可求得下元素是否在数组范围内，    
                 * 如果在数组范围内，则继续循环，直到索引超现数组范围    
                 */      
                while ((endIndex + step) <= end) {       
                    endIndex += step;       
                }       
              
                // i为每小组里的第二个元素开始       
                for (int i = groupIndex + step; i <= end; i += step) {       
                    for (int j = groupIndex; j < i; j += step) {       
                        Integer insertedElem = array[i];       
                        //从有序数组中最一个元素开始查找第一个大于待插入的元素       
                        if ((array[j].compareTo(insertedElem)) >= 0) {       
                            //找到插入点后，从插入点开始向后所有元素后移一位       
                            move(array, j, i - step, step);       
                            array[j] = insertedElem;       
                            break;       
                        }       
                    }       
                }       
            }       
              
            /**    
             * 根据数组长度求初始步长    
             *     
             * 我们选择步长的公式为：2^k-1,2^(k-1)-1,...,15,7,3,1 ，其中2^k 减一即为该步长序列，k    
             * 为排序轮次    
             *     
             * 初始步长：step = 2^k-1     
             * 初始步长约束条件：step < len - 1 初始步长的值要小于数组长度还要减一的值（因    
             * 为第一轮分组时尽量不要分为一组，除非数组本身的长度就小于等于4）    
             *     
             * 由上面两个关系试可以得知：2^k - 1 < len - 1 关系式，其中k为轮次，如果把 2^k 表 达式    
             * 转换成 step 表达式，则 2^k-1 可使用 (step + 1)*2-1 替换（因为 step+1 相当于第k-1    
             * 轮的步长，所以在 step+1 基础上乘以 2 就相当于 2^k 了），即步长与数组长度的关系不等式为    
             * (step + 1)*2 - 1 < len -1    
             *     
             * @param len 数组长度    
             * @return    
             */      
            public static int initialStep(int len) {       
                /*    
                 * 初始值设置为步长公式中的最小步长，从最小步长推导出最长初始步长值，即按照以下公式来推:    
                 * 1,3,7,15,...,2^(k-1)-1,2^k-1    
                 * 如果数组长度小于等于4时，步长为1，即长度小于等于4的数组不用分组，此时直接退化为直接插入排序    
                 */      
                int step = 1;       
              
                //试探下一个步长是否满足条件，如果满足条件，则步长置为下一步长       
                while ((step + 1) * 2 - 1 < len - 1) {       
                    step = (step + 1) * 2 - 1;       
                }       
              
                System.out.println("初始步长 : " + step);       
                return step;       
            }       
                
            /**    
             * 以指定的步长将数组元素后移，步长指定每个元素间的间隔    
             * @param array 待排序数组    
             * @param startIndex 从哪里开始移    
             * @param endIndex 到哪个元素止    
             * @param step 步长    
             */      
            protected final void move(Integer[] array, int startIndex, int endIndex, int step)  {       
                for (int i = endIndex; i >= startIndex; i -= step) {       
                    array[i + step] = array[i];       
                }       
            }       
            
                
            /**    
             * 测试    
             * @param args    
             */      
            public static void main(String[] args) {       
                Integer[] intgArr = { 5, 9, 1, 4, 8, 2, 6, 3, 7, 0 };       
                ShellSort shellSort = new ShellSort();       
                shellSort.sort(intgArr,0,intgArr.length-1);    
                for(Integer intObj:intgArr){    
                    System.out.print(intObj + " ");    
                }    
            }       
        }     

五、快速排序（Quick Sort）
1. 基本思想：
　　在当前无序区R[1..H]中任取一个数据元素作为比较的"基准"(不妨记为X)，用此基准将当前无序区划分为左右两个较小的无序区：R[1..I-1]和R[I+1..H]，且左边的无序子区中数据元素均小于等于基准元素，右边的无序子区中数据元素均大于等于基准元素，而基准X则位于最终排序的位置上，即R[1..I-1]≤X.Key≤R[I+1..H](1≤I≤H)，当R[1..I-1]和R[I+1..H]均非空时，分别对它们进行上述的划分过程，直至所有无序子区中的数据元素均已排序为止。
2. 排序过程：

java代码实现：


/**  
    * 快速排序：  
    */    
        
    public class QuickSort {       
          
        /**    
         * 排序算法的实现，对数组中指定的元素进行排序    
         * @param array 待排序的数组    
         * @param from 从哪里开始排序    
         * @param end 排到哪里    
         * @param c 比较器    
         */      
        //定义了一个这样的公有方法sort，在这个方法体里面执行私有方法quckSort（这种方式值得借鉴）。    
        public void sort(Integer[] array, int from, int end) {       
            quickSort(array, from, end);       
        }       
          
        /**    
         * 递归快速排序实现    
         * @param array 待排序数组    
         * @param low 低指针    
         * @param high 高指针    
         * @param c 比较器    
         */      
        private void quickSort(Integer[] array, int low, int high) {       
            /*    
             * 如果分区中的低指针小于高指针时循环；如果low=higth说明数组只有一个元素，无需再处理；    
             * 如果low > higth，则说明上次枢纽元素的位置pivot就是low或者是higth，此种情况    
             * 下分区不存，也不需处理    
             */      
            if (low < high) {       
                //对分区进行排序整理       
                    
                //int pivot = partition1(array, low, high);    
                int pivot = partition2(array, low, high);       
                //int pivot = partition3(array, low, high);          
                    
                /*    
                 * 以pivot为边界，把数组分成三部分[low, pivot - 1]、[pivot]、[pivot + 1, high]    
                 * 其中[pivot]为枢纽元素，不需处理，再对[low, pivot - 1]与[pivot + 1, high]    
                 * 各自进行分区排序整理与进一步分区    
                 */      
                quickSort(array, low, pivot - 1);       
                quickSort(array, pivot + 1, high);       
            }       
          
        }       
          
        /**    
         * 实现一    
         *     
         * @param array 待排序数组    
         * @param low 低指针    
         * @param high 高指针    
         * @param c 比较器    
         * @return int 调整后中枢位置    
         */      
        private int partition1(Integer[] array, int low, int high) {       
            Integer pivotElem = array[low];//以第一个元素为中枢元素       
            //从前向后依次指向比中枢元素小的元素，刚开始时指向中枢，也是小于与大小中枢的元素的分界点       
            int border = low;       
          
            /*    
             * 在中枢元素后面的元素中查找小于中枢元素的所有元素，并依次从第二个位置从前往后存放    
             * 注，这里最好使用i来移动，如果直接移动low的话，最后不知道数组的边界了，但后面需要    
             * 知道数组的边界    
             */      
            for (int i = low + 1; i <= high; i++) {       
                //如果找到一个比中枢元素小的元素       
                if ((array[i].compareTo(pivotElem)) < 0) {       
                    swap(array, ++border, i);//border前移，表示有小于中枢元素的元素       
                }       
            }       
            /*    
             * 如果border没有移动时说明说明后面的元素都比中枢元素要大，border与low相等，此时是    
             * 同一位置交换，是否交换都没关系；当border移到了high时说明所有元素都小于中枢元素，此    
             * 时将中枢元素与最后一个元素交换即可，即low与high进行交换，大的中枢元素移到了 序列最    
             * 后；如果 low <minIndex< high，表 明中枢后面的元素前部分小于中枢元素，而后部分大于    
             * 中枢元素，此时中枢元素与前部分数组中最后一个小于它的元素交换位置，使得中枢元素放置在    
             * 正确的位置    
             */      
            swap(array, border, low);       
            return border;       
        }       
          
        /**    
         * 实现二    
         *     
         * @param array 待排序数组    
         * @param low 待排序区低指针    
         * @param high 待排序区高指针    
         * @param c 比较器    
         * @return int 调整后中枢位置    
         */      
        private int partition2(Integer[] array, int low, int high) {       
            int pivot = low;//中枢元素位置，我们以第一个元素为中枢元素       
            //退出条件这里只可能是 low = high       
            while (true) {       
                if (pivot != high) {//如果中枢元素在低指针位置时，我们移动高指针       
                    //如果高指针元素小于中枢元素时，则与中枢元素交换       
                    if ((array[high].compareTo(array[pivot])) < 0) {       
                        swap(array, high, pivot);       
                        //交换后中枢元素在高指针位置了       
                        pivot = high;       
                    } else {//如果未找到小于中枢元素，则高指针前移继续找       
                        high--;       
                    }       
                } else {//否则中枢元素在高指针位置       
                    //如果低指针元素大于中枢元素时，则与中枢元素交换       
                    if ((array[low].compareTo(array[pivot])) > 0) {       
                        swap(array, low, pivot);       
                        //交换后中枢元素在低指针位置了       
                        pivot = low;       
                    } else {//如果未找到大于中枢元素，则低指针后移继续找       
                        low++;       
                    }       
                }       
                if (low == high) {       
                    break;       
                }       
            }       
            //返回中枢元素所在位置，以便下次分区       
            return pivot;       
        }       
          
        /**    
         * 实现三    
         *     
         * @param array 待排序数组    
         * @param low 待排序区低指针    
         * @param high 待排序区高指针    
         * @param c 比较器    
         * @return int 调整后中枢位置    
         */      
        private int partition3(Integer[] array, int low, int high) {       
            int pivot = low;//中枢元素位置，我们以第一个元素为中枢元素       
            low++;       
            //----调整高低指针所指向的元素顺序，把小于中枢元素的移到前部分，大于中枢元素的移到后面部分       
            //退出条件这里只可能是 low = high       
          
            while (true) {       
                //如果高指针未超出低指针       
                while (low < high) {       
                    //如果低指针指向的元素大于或等于中枢元素时表示找到了，退出，注：等于时也要后移       
                    if ((array[low].compareTo(array[pivot])) >= 0) {       
                        break;       
                    } else {//如果低指针指向的元素小于中枢元素时继续找       
                        low++;       
                    }       
                }       
          
                while (high > low) {       
                    //如果高指针指向的元素小于中枢元素时表示找到，退出       
                    if ((array[high].compareTo(array[pivot])) < 0) {       
                        break;       
                    } else {//如果高指针指向的元素大于中枢元素时继续找       
                        high--;       
                    }       
                }       
                //退出上面循环时 low = high       
                if (low == high) {       
                    break;       
                }       
          
                swap(array, low, high);       
            }       
          
            //----高低指针所指向的元素排序完成后，还得要把中枢元素放到适当的位置       
            if ((array[pivot].compareTo(array[low])) > 0) {       
                //如果退出循环时中枢元素大于了低指针或高指针元素时，中枢元素需与low元素交换       
                swap(array, low, pivot);       
                pivot = low;       
            } else if ((array[pivot].compareTo(array[low])) <= 0) {       
                swap(array, low - 1, pivot);       
                pivot = low - 1;       
            }       
          
            //返回中枢元素所在位置，以便下次分区       
            return pivot;       
        }       
            
            /**    
         * 交换数组中的两个元素的位置    
         * @param array 待交换的数组    
         * @param i 第一个元素    
         * @param j 第二个元素    
         */      
        public void swap(Integer[] array, int i, int j) {       
            if (i != j) {//只有不是同一位置时才需交换       
                Integer tmp = array[i];       
                array[i] = array[j];       
                array[j] = tmp;       
            }       
        }     
            
        /**     
        * 测试     
        * @param args     
        */      
        public static void main(String[] args) {       
            Integer[] intgArr = { 5, 9, 1, 4, 2, 6, 3, 8, 0, 7 };      
            QuickSort quicksort = new QuickSort();       
            quicksort.sort(intgArr,0,intgArr.length-1);    
            for(Integer intObj:intgArr){    
                System.out.print(intObj + " ");    
            }    
        }       
    }   

六、归并排序
java代码实现：view plai



/**   
        归并排序：里面是一个递归程序，深刻理解之。   
    */    
    public class MergeSort{       
          
        /**    
         * 递归划分数组    
         * @param arr    
         * @param from    
         * @param end    
         * @param c void    
         */      
        public void partition(Integer[] arr, int from, int end) {       
            //划分到数组只有一个元素时才不进行再划分       
            if (from < end) {       
                //从中间划分成两个数组       
                int mid = (from + end) / 2;       
                partition(arr, from, mid);       
                partition(arr, mid + 1, end);       
                //合并划分后的两个数组       
                merge(arr, from, end, mid);       
            }       
        }       
          
        /**    
         * 数组合并，合并过程中对两部分数组进行排序    
         * 前后两部分数组里是有序的    
         * @param arr    
         * @param from    
         * @param end    
         * @param mid    
         * @param c void    
         */      
        public void merge(Integer[] arr, int from, int end, int mid) {       
            Integer[] tmpArr = new Integer[10];    
            int tmpArrIndex = 0;//指向临时数组       
            int part1ArrIndex = from;//指向第一部分数组       
            int part2ArrIndex = mid + 1;//指向第二部分数组       
          
            //由于两部分数组里是有序的，所以每部分可以从第一个元素依次取到最后一个元素，再对两部分       
            //取出的元素进行比较。只要某部分数组元素取完后，退出循环       
            while ((part1ArrIndex <= mid) && (part2ArrIndex <= end)) {       
                //从两部分数组里各取一个进行比较，取最小一个并放入临时数组中       
                if (arr[part1ArrIndex] - arr[part2ArrIndex] < 0) {       
                    //如果第一部分数组元素小，则将第一部分数组元素放入临时数组中，并且临时数组指针       
                    //tmpArrIndex下移一个以做好下次存储位置准备，前部分数组指针part1ArrIndex       
                    //也要下移一个以便下次取出下一个元素与后部分数组元素比较       
                    tmpArr[tmpArrIndex++] = arr[part1ArrIndex++];       
                } else {       
                    //如果第二部分数组元素小，则将第二部分数组元素放入临时数组中       
                    tmpArr[tmpArrIndex++] = arr[part2ArrIndex++];       
                }       
            }       
            //由于退出循环后，两部分数组中可能有一个数组元素还未处理完，所以需要额外的处理，当然不可       
            //能两部分数组都有未处理完的元素，所以下面两个循环最多只有一个会执行，并且都是大于已放入       
            //临时数组中的元素       
            while (part1ArrIndex <= mid) {       
                tmpArr[tmpArrIndex++] = arr[part1ArrIndex++];       
            }       
            while (part2ArrIndex <= end) {       
                tmpArr[tmpArrIndex++] = arr[part2ArrIndex++];       
            }       
          
            //最后把临时数组拷贝到源数组相同的位置       
            System.arraycopy(tmpArr, 0, arr, from, end - from + 1);       
        }       
          
        /**    
         * 测试    
         * @param args    
         */      
        public static void main(String[] args) {       
            Integer[] intgArr = {5,9,1,4,2,6,3,8,0,7};       
            MergeSort insertSort = new MergeSort();       
            insertSort.partition(intgArr,0,intgArr.length-1);    
            for(Integer a:intgArr){    
                System.out.print(a + " ");    
            }    
        }       
    }         

copy
七、堆排序(Heap Sort)
1. 基本思想：
  堆排序是一树形选择排序，在排序过程中，将R[1..N]看成是一颗完全二叉树的顺序存储结构，利用完全二叉树中双亲结点和孩子结点之间的内在关系来选择最小的元素。
2. 堆的定义: N个元素的序列K1,K2,K3,...,Kn.称为堆，当且仅当该序列满足特性：
       Ki≤K2i Ki ≤K2i+1(1≤ I≤ [N/2])

  堆实质上是满足如下性质的完全二叉树：树中任一非叶子结点的关键字均大于等于其孩子结点的关键字。例如序列10,15,56,25,30,70就是一个堆，它对应的完全二叉树如上图所示。这种堆中根结点（称为堆顶）的关键字最小，我们把它称为小根堆。反之，若完全二叉树中任一非叶子结点的关键字均大于等于其孩子的关键字，则称之为大根堆。
3. 排序过程：
堆排序正是利用小根堆（或大根堆）来选取当前无序区中关键字小（或最大）的记录实现排序的。我们不妨利用大根堆来排序。每一趟排序的基本操作是：将当前无序区调整为一个大根堆，选取关键字最大的堆顶记录，将它和无序区中的最后一个记录交换。这样，正好和直接选择排序相反，有序区是在原记录区的尾部形成并逐步向前扩大到整个记录区。
【示例】：对关键字序列42，13，91，23，24，16，05，88建堆 
java代码实现：vi
/**  
    * 选择排序之堆排序：  
    */    
    public class HeapSort {       
          
        /**    
         * 排序算法的实现，对数组中指定的元素进行排序    
         * @param array 待排序的数组    
         * @param from 从哪里开始排序    
         * @param end 排到哪里    
         * @param c 比较器    
         */      
        public void sort(Integer[] array, int from, int end) {       
            //创建初始堆       
            initialHeap(array, from, end);       
          
            /*    
             * 对初始堆进行循环，且从最后一个节点开始，直接树只有两个节点止    
             * 每轮循环后丢弃最后一个叶子节点，再看作一个新的树    
             */      
            for (int i = end - from + 1; i >= 2; i--) {       
                //根节点与最后一个叶子节点交换位置，即数组中的第一个元素与最后一个元素互换       
                swap(array, from, i - 1);       
                //交换后需要重新调整堆       
                adjustNote(array, 1, i - 1);       
            }       
          
        }       
          
        /**    
         * 初始化堆    
         * 比如原序列为：7,2,4,3,12,1,9,6,8,5,10,11    
         * 则初始堆为：1,2,4,3,5,7,9,6,8,12,10,11    
         * @param arr 排序数组    
         * @param from 从哪    
         * @param end 到哪    
         * @param c 比较器    
         */      
        private void initialHeap(Integer[] arr, int from, int end) {       
            int lastBranchIndex = (end - from + 1) / 2;//最后一个非叶子节点       
            //对所有的非叶子节点进行循环 ，且从最一个非叶子节点开始       
            for (int i = lastBranchIndex; i >= 1; i--) {       
                adjustNote(arr, i, end - from + 1);       
            }       
        }       
          
        /**    
         * 调整节点顺序，从父、左右子节点三个节点中选择一个最大节点与父节点转换    
         * @param arr 待排序数组    
         * @param parentNodeIndex 要调整的节点，与它的子节点一起进行调整    
         * @param len 树的节点数    
         * @param c 比较器    
         */      
        private void adjustNote(Integer[] arr, int parentNodeIndex, int len) {       
            int minNodeIndex = parentNodeIndex;       
            //如果有左子树，i * 2为左子节点索引        
            if (parentNodeIndex * 2 <= len) {       
                //如果父节点小于左子树时        
                if ((arr[parentNodeIndex - 1].compareTo(arr[parentNodeIndex * 2 - 1])) < 0) {       
                    minNodeIndex = parentNodeIndex * 2;//记录最大索引为左子节点索引        
                }       
          
                // 只有在有或子树的前提下才可能有右子树，再进一步断判是否有右子树        
                if (parentNodeIndex * 2 + 1 <= len) {       
                    //如果右子树比最大节点更大        
                    if ((arr[minNodeIndex - 1].compareTo(arr[(parentNodeIndex * 2 + 1) - 1])) < 0) {       
                        minNodeIndex = parentNodeIndex * 2 + 1;//记录最大索引为右子节点索引        
                    }       
                }       
            }       
          
            //如果在父节点、左、右子节点三都中，最大节点不是父节点时需交换，把最大的与父节点交换，创建大顶堆       
            if (minNodeIndex != parentNodeIndex) {       
                swap(arr, parentNodeIndex - 1, minNodeIndex - 1);       
                //交换后可能需要重建堆，原父节点可能需要继续下沉       
                if (minNodeIndex * 2 <= len) {//是否有子节点，注，只需判断是否有左子树即可知道       
                    adjustNote(arr, minNodeIndex, len);       
                }       
            }       
        }       
            
                /**    
         * 交换数组中的两个元素的位置    
         * @param array 待交换的数组    
         * @param i 第一个元素    
         * @param j 第二个元素    
         */      
        public void swap(Integer[] array, int i, int j) {       
            if (i != j) {//只有不是同一位置时才需交换       
                Integer tmp = array[i];       
                array[i] = array[j];       
                array[j] = tmp;       
            }       
        }     
            
        /**     
        * 测试     
        * @param args     
        */      
        public static void main(String[] args) {       
            Integer[] intgArr = { 5, 9, 1, 4, 2, 6, 3, 8, 0, 7 };      
            HeapSort heapsort = new HeapSort();       
            heapsort.sort(intgArr,0,intgArr.length-1);    
            for(Integer intObj:intgArr){    
                System.out.print(intObj + " ");    
            }    
        }       
          
    }      



 
八、桶式排序
java代码实现:


/**    
     * 桶式排序:    
     * 桶式排序不再是基于比较的了，它和基数排序同属于分配类的排序，    
     * 这类排序的特点是事先要知道待排 序列的一些特征。    
     * 桶式排序事先要知道待排 序列在一个范围内，而且这个范围应该不是很大的。    
     * 比如知道待排序列在[0,M）内，那么可以分配M个桶，第I个桶记录I的出现情况，    
     * 最后根据每个桶收到的位置信息把数据输出成有序的形式。    
     * 这里我们用两个临时性数组，一个用于记录位置信息，一个用于方便输出数据成有序方式，    
     * 另外我们假设数据落在0到MAX,如果所给数据不是从0开始，你可以把每个数减去最小的数。    
     */      
    public class BucketSort {       
         public void sort(int[] keys,int from,int len,int max)       
            {       
                int[] temp=new int[len];       
                int[] count=new int[max];       
                       
                       
                for(int i=0;i<len;i++)       
                {       
                    count[keys[from+i]]++;       
                }       
                //calculate position info       
                for(int i=1;i<max;i++)       
                {       
                    count[i]=count[i]+count[i-1];//这意味着有多少数目小于或等于i，因此它也是position+ 1       
                }       
                       
                System.arraycopy(keys, from, temp, 0, len);       
                for(int k=len-1;k>=0;k--)//从最末到开头保持稳定性       
                {       
                    keys[--count[temp[k]]]=temp[k];// position +1 =count       
                }       
            }       
            /**    
             * @param args    
             */      
            public static void main(String[] args) {       
          
                int[] a={1,4,8,3,2,9,5,0,7,6,9,10,9,13,14,15,11,12,17,16};       
                BucketSort bucketSort=new BucketSort();       
                bucketSort.sort(a,0,a.length,20);//actually is 18, but 20 will also work       
                       
                       
                for(int i=0;i<a.length;i++)       
                {       
                    System.out.print(a[i]+",");       
                }       
          
            }       
          
          
    }         

      
九、基数排序
java代码实现：


/**  
    * 基数排序：  
    */    
    public class RadixSort {       
          
        /**    
         * 取数x上的第d位数字    
         * @param x 数    
         * @param d 第几位，从低位到高位    
         * @return    
         */      
        public int digit(long x, long d) {       
          
            long pow = 1;       
            while (--d > 0) {       
                pow *= 10;       
            }       
            return (int) (x / pow % 10);       
        }       
          
        /**    
         * 基数排序实现，以升序排序（下面程序中的位记录器count中，从第0个元素到第9个元素依次用来    
         * 记录当前比较位是0的有多少个..是9的有多少个数，而降序时则从第0个元素到第9个元素依次用来    
         * 记录当前比较位是9的有多少个..是0的有多少个数）    
         * @param arr 待排序数组    
         * @param digit 数组中最大数的位数    
         * @return    
         */      
        public long[] radixSortAsc(long[] arr) {       
            //从低位往高位循环       
            for (int d = 1; d <= getMax(arr); d++) {       
                //临时数组，用来存放排序过程中的数据       
                long[] tmpArray = new long[arr.length];       
                //位记数器，从第0个元素到第9个元素依次用来记录当前比较位是0的有多少个..是9的有多少个数       
                int[] count = new int[10];       
                //开始统计0有多少个，并存储在第0位，再统计1有多少个，并存储在第1位..依次统计到9有多少个       
                for (int i = 0; i < arr.length; i++) {       
                    count[digit(arr[i], d)] += 1;       
                }       
                /*    
                 * 比如某次经过上面统计后结果为：[0, 2, 3, 3, 0, 0, 0, 0, 0, 0]则经过下面计算后 结果为：    
                 * [0, 2, 5, 8, 8, 8, 8, 8, 8, 8]但实质上只有如下[0, 2, 5, 8, 0, 0, 0, 0, 0, 0]中    
                 * 非零数才用到，因为其他位不存在，它们分别表示如下：2表示比较位为1的元素可以存放在索引为1、0的    
                 * 位置，5表示比较位为2的元素可以存放在4、3、2三个(5-2=3)位置，8表示比较位为3的元素可以存放在    
                 * 7、6、5三个(8-5=3)位置    
                 */      
                for (int i = 1; i < 10; i++) {       
                    count[i] += count[i - 1];       
                }       
          
                /*    
                 * 注，这里只能从数组后往前循环，因为排序时还需保持以前的已排序好的 顺序，不应该打    
                 * 乱原来已排好的序，如果从前往后处理，则会把原来在前面会摆到后面去，因为在处理某个    
                 * 元素的位置时，位记数器是从大到到小（count[digit(arr[i], d)]--）的方式来处    
                 * 理的，即先存放索引大的元素，再存放索引小的元素，所以需从最后一个元素开始处理。    
                 * 如有这样的一个序列[212,213,312]，如果按照从第一个元素开始循环的话，经过第一轮    
                 * 后（个位）排序后，得到这样一个序列[312,212,213]，第一次好像没什么问题，但问题会    
                 * 从第二轮开始出现，第二轮排序后，会得到[213,212,312]，这样个位为3的元素本应该    
                 * 放在最后，但经过第二轮后却排在了前面了，所以出现了问题    
                 */      
                for (int i = arr.length - 1; i >= 0; i--) {//只能从最后一个元素往前处理       
                    //for (int i = 0; i < arr.length; i++) {//不能从第一个元素开始循环       
                    tmpArray[count[digit(arr[i], d)] - 1] = arr[i];       
                    count[digit(arr[i], d)]--;       
                }       
          
                System.arraycopy(tmpArray, 0, arr, 0, tmpArray.length);       
            }       
            return arr;       
        }       
          
        /**    
         * 基数排序实现，以降序排序（下面程序中的位记录器count中，从第0个元素到第9个元素依次用来    
         * 记录当前比较位是0的有多少个..是9的有多少个数，而降序时则从第0个元素到第9个元素依次用来    
         * 记录当前比较位是9的有多少个..是0的有多少个数）    
         * @param arr 待排序数组    
         * @return    
         */      
        public long[] radixSortDesc(long[] arr) {       
            for (int d = 1; d <= getMax(arr); d++) {       
                long[] tmpArray = new long[arr.length];       
                //位记数器，从第0个元素到第9个元素依次用来记录当前比较位是9的有多少个..是0的有多少个数       
                int[] count = new int[10];       
                //开始统计0有多少个，并存储在第9位，再统计1有多少个，并存储在第8位..依次统计       
                //到9有多少个，并存储在第0位       
                for (int i = 0; i < arr.length; i++) {       
                    count[9 - digit(arr[i], d)] += 1;       
                }       
          
                for (int i = 1; i < 10; i++) {       
                    count[i] += count[i - 1];       
                }       
          
                for (int i = arr.length - 1; i >= 0; i--) {       
                    tmpArray[count[9 - digit(arr[i], d)] - 1] = arr[i];       
                    count[9 - digit(arr[i], d)]--;       
                }       
          
                System.arraycopy(tmpArray, 0, arr, 0, tmpArray.length);       
            }       
            return arr;       
        }       
          
        private int getMax(long[] array) {       
            int maxlIndex = 0;       
            for (int j = 1; j < array.length; j++) {       
                if (array[j] > array[maxlIndex]) {       
                    maxlIndex = j;       
                }       
            }       
            return String.valueOf(array[maxlIndex]).length();       
        }       
          
        public static void main(String[] args) {       
            long[] ary = new long[] { 123, 321, 132, 212, 213, 312, 21, 223 };       
            RadixSort rs = new RadixSort();       
            System.out.println("升 - " + Arrays.toString(rs.radixSortAsc(ary)));       
          
            ary = new long[] { 123, 321, 132, 212, 213, 312, 21, 223 };       
            System.out.println("降 - " + Arrays.toString(rs.radixSortDesc(ary)));       
        }       
    }     


 
十、几种排序算法的比较和选择 
1. 选取排序方法需要考虑的因素：
(1) 待排序的元素数目n；
(2) 元素本身信息量的大小；
(3) 关键字的结构及其分布情况；
(4) 语言工具的条件，辅助空间的大小等。
2. 小结：
(1) 若n较小(n <= 50)，则可以采用直接插入排序或直接选择排序。由于直接插入排序所需的记录移动操作较直接选择排序多，因而当记录本身信息量较大时，用直接选择排序较好。
(2) 若文件的初始状态已按关键字基本有序，则选用直接插入或冒泡排序为宜。
(3) 若n较大，则应采用时间复杂度为O(nlog2n)的排序方法：快速排序、堆排序或归并排序。 快速排序是目前基于比较的内部排序法中被认为是最好的方法。
(4) 在基于比较排序方法中，每次比较两个关键字的大小之后，仅仅出现两种可能的转移，因此可以用一棵二叉树来描述比较判定过程，由此可以证明：当文件的n个关键字随机分布时，任何借助于"比较"的排序算法，至少需要O(nlog2n)的时间。
(5) 当记录本身信息量较大时，为避免耗费大量时间移动记录，可以用链表作为存储结构。
排序简介 
排序是数据处理中经常使用的一种重要运算,在计算机及其应用系统中,花费在排序上的时间在系统运行时间中占有很大比重;并且排序本身对推动算法分析的发展也起很大作用。目前已有上百种排序方法，但尚未有一个最理想的尽如人意的方法，本章介绍常用的如下排序方法，并对它们进行分析和比较。

1、插入排序（直接插入排序、折半插入排序、希尔排序）；
2、交换排序（起泡排序、快速排序）；
3、选择排序（直接选择排序、堆排序）；
4、归并排序；
5、基数排序；

学习重点 
1、掌握排序的基本概念和各种排序方法的特点，并能加以灵活应用；
2、掌握插入排序(直接插入排序、折半插入排序、希尔排序)、交换排序（起泡排序、快速排序）、选择排序（直接选择排序、堆排序）、二路归并排序的方法及其性能分析方法；
3、了解基数排序方法及其性能分析方法。

排序（sort）或分类

    　所谓排序，就是要整理文件中的记录，使之按关键字递增(或递减)次序排列起来。其确切定义如下：
　　输入：n个记录R1，R2，…，Rn，其相应的关键字分别为K1，K2，…，Kn。
　　输出：Ril，Ri2，…，Rin，使得Ki1≤Ki2≤…≤Kin。(或Ki1≥Ki2≥…≥Kin)。

1．被排序对象--文件
　　被排序的对象--文件由一组记录组成。
　　记录则由若干个数据项(或域)组成。其中有一项可用来标识一个记录，称为关键字项。该数据项的值称为关键字(Key)。
  注意：
　    在不易产生混淆时，将关键字项简称为关键字。

2．排序运算的依据--关键字
    　用来作排序运算依据的关键字，可以是数字类型，也可以是字符类型。
　    关键字的选取应根据问题的要求而定。
【例】在高考成绩统计中将每个考生作为一个记录。每条记录包含准考证号、姓名、各科的分数和总分数等项内容。若要惟一地标识一个考生的记录，则必须用"准考证号"作为关键字。若要按照考生的总分数排名次，则需用"总分数"作为关键字。

排序的稳定性

　    当待排序记录的关键字均不相同时，排序结果是惟一的，否则排序结果不唯一。
    　在待排序的文件中，若存在多个关键字相同的记录，经过排序后这些具有相同关键字的记录之间的相对次序保持不变，该排序方法是稳定的；若具有相同关键字的记录之间的相对次序发生变化，则称这种排序方法是不稳定的。
  注意：
　    排序算法的稳定性是针对所有输入实例而言的。即在所有可能的输入实例中，只要有一个实例使得算法不满足稳定性要求，则该排序算法就是不稳定的。

排序方法的分类

1．按是否涉及数据的内、外存交换分
    　在排序过程中，若整个文件都是放在内存中处理，排序时不涉及数据的内、外存交换，则称之为内部排序(简称内排序)；反之，若排序过程中要进行数据的内、外存交换，则称之为外部排序。
  注意：
　    ① 内排序适用于记录个数不很多的小文件
    　② 外排序则适用于记录个数太多，不能一次将其全部记录放人内存的大文件。

2．按策略划分内部排序方法
    　可以分为五类：插入排序、选择排序、交换排序、归并排序和分配排序。
排序算法分析

1．排序算法的基本操作
    　大多数排序算法都有两个基本的操作：
　　(1) 比较两个关键字的大小；
　　(2) 改变指向记录的指针或移动记录本身。
  注意：
　    第(2)种基本操作的实现依赖于待排序记录的存储方式。

2．待排文件的常用存储方式
（1） 以顺序表(或直接用向量)作为存储结构
    排序过程：对记录本身进行物理重排（即通过关键字之间的比较判定，将记录移到合适的位置）

（2） 以链表作为存储结构
　　排序过程：无须移动记录，仅需修改指针。通常将这类排序称为链表(或链式)排序；

（3） 用顺序的方式存储待排序的记录，但同时建立一个辅助表(如包括关键字和指向记录位置的指针组成的索引表)
　　排序过程：只需对辅助表的表目进行物理重排（即只移动辅助表的表目，而不移动记录本身）。适用于难于在链表上实现，仍需避免排序过程中移动记录的排序方法。

3．排序算法性能评价
（1） 评价排序算法好坏的标准
　　评价排序算法好坏的标准主要有两条：
    　① 执行时间和所需的辅助空间
    　② 算法本身的复杂程度

（2） 排序算法的空间复杂度
　　若排序算法所需的辅助空间并不依赖于问题的规模n，即辅助空间是O(1)，则称之为就地排序(In-PlaceSou)。
　　非就地排序一般要求的辅助空间为O(n)。

（3） 排序算法的时间开销
　　大多数排序算法的时间开销主要是关键字之间的比较和记录的移动。有的排序算法其执行时间不仅依赖于问题的规模，还取决于输入实例中数据的状态。

文件的顺序存储结构表示

 define n l00 //假设的文件长度，即待排序的记录数目
  typedef int KeyType； //假设的关键字类型
  typedef struct{ //记录类型
    KeyType key； //关键字项
    InfoType otherinfo；//其它数据项，类型InfoType依赖于具体应用而定义
   }RecType；
  typedef RecType SeqList[n+1]；//SeqList为顺序表类型，表中第0个单元一般用作哨兵
  注意：
    　若关键字类型没有比较算符，则可事先定义宏或函数来表示比较运算。
【例】关键字为字符串时，可定义宏"#define LT(a，b)(Stromp((a)，(b))<0)"。那么算法中"a<b"可用"LT(a，b)"取代。若使用C++，则定义重载的算符"<"更为方便。
按平均时间将排序分为四类：

（1）平方阶(O(n2))排序
    　一般称为简单排序，例如直接插入、直接选择和冒泡排序；

（2）线性对数阶(O(nlgn))排序
    　如快速、堆和归并排序；

（3）O(n1+￡)阶排序
    　￡是介于0和1之间的常数，即0<￡<1，如希尔排序；

（4）线性阶(O(n))排序
    　如桶、箱和基数排序。

各种排序方法比较

     简单排序中直接插入最好，快速排序最快，当文件为正序时，直接插入和冒泡均最佳。

影响排序效果的因素

    　因为不同的排序方法适应不同的应用环境和要求，所以选择合适的排序方法应综合考虑下列因素：
　　①待排序的记录数目n；
　　②记录的大小(规模)；
　　③关键字的结构及其初始状态；
　　④对稳定性的要求；
　　⑤语言工具的条件；
　　⑥存储结构；
　　⑦时间和辅助空间复杂度等。

不同条件下，排序方法的选择

(1)若n较小(如n≤50)，可采用直接插入或直接选择排序。
    　当记录规模较小时，直接插入排序较好；否则因为直接选择移动的记录数少于直接插人，应选直接选择排序为宜。
(2)若文件初始状态基本有序(指正序)，则应选用直接插人、冒泡或随机的快速排序为宜；
(3)若n较大，则应采用时间复杂度为O(nlgn)的排序方法：快速排序、堆排序或归并排序。
    　快速排序是目前基于比较的内部排序中被认为是最好的方法，当待排序的关键字是随机分布时，快速排序的平均时间最短；
    　堆排序所需的辅助空间少于快速排序，并且不会出现快速排序可能出现的最坏情况。这两种排序都是不稳定的。
    　若要求排序稳定，则可选用归并排序。但本章介绍的从单个记录起进行两两归并的  排序算法并不值得提倡，通常可以将它和直接插入排序结合在一起使用。先利用直接插入排序求得较长的有序子文件，然后再两两归并之。因为直接插入排序是稳定的，所以改进后的归并排序仍是稳定的。
4)在基于比较的排序方法中，每次比较两个关键字的大小之后，仅仅出现两种可能的转移，因此可以用一棵二叉树来描述比较判定过程。
    　当文件的n个关键字随机分布时，任何借助于"比较"的排序算法，至少需要O(nlgn)的时间。
    　箱排序和基数排序只需一步就会引起m种可能的转移，即把一个记录装入m个箱子之一，因此在一般情况下，箱排序和基数排序可能在O(n)时间内完成对n个记录的排序。但是，箱排序和基数排序只适用于像字符串和整数这类有明显结构特征的关键字，而当关键字的取值范围属于某个无穷集合(例如实数型关键字)时，无法使用箱排序和基数排序，这时只有借助于"比较"的方法来排序。
    　若n很大，记录的关键字位数较少且可以分解时，采用基数排序较好。虽然桶排序对关键字的结构无要求，但它也只有在关键字是随机分布时才能使平均时间达到线性阶，否则为平方阶。同时要注意，箱、桶、基数这三种分配排序均假定了关键字若为数字时，则其值均是非负的，否则将其映射到箱(桶)号时，又要增加相应的时间。
(5)有的语言(如Fortran，Cobol或Basic等)没有提供指针及递归，导致实现归并、快速(它们用递归实现较简单)和基数(使用了指针)等排序算法变得复杂。此时可考虑用其它排序。
(6)本章给出的排序算法，输人数据均是存储在一个向量中。当记录的规模较大时，为避免耗费大量的时间去移动记录，可以用链表作为存储结构。譬如插入排序、归并排序、基数排序都易于在链表上实现，使之减少记录的移动次数。但有的排序方法，如快速排序和堆排序，在链表上却难于实现，在这种情况下，可以提取关键字建立索引表，然后对索引表进行排序。然而更为简单的方法是：引人一个整型向量t作为辅助表，排序前令t[i]=i(0≤i<n)，若排序算法中要求交换R[i]和R[j]，则只需交换t[i]和t[j]即可；排序结束后，向量t就指示了记录之间的顺序关系：
        R[t[0]].key≤R[t[1]].key≤…≤R[t[n-1]].key
  若要求最终结果是：
       R[0].key≤R[1].key≤…≤R[n-1].key
则可以在排序结束后，再按辅助表所规定的次序重排各记录，完成这种重排的时间是O(n)。


 


