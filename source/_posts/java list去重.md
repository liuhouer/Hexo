
title: "java list去重"
date: 2015-12-15 11:09:22
tags: [java,list去重]
categories: java
---

java list去重：
①通过Iterator 的remove方法；
②直接将结果赋值给另一个List；
③用set去重

Talk is cheap,show me your code ~
<!--more-->

①
	
	/**
		 * list去重-通过Iterator 的remove方法；
		 * @return
		 */
		public static  List<String> listRM1(List<String> list) {  
		    List<String> listTemp= new ArrayList<String>();  
			 Iterator<String> it=list.iterator();  
			 while(it.hasNext()){  
			  String a=it.next();  
			  if(listTemp.contains(a)){  
			   it.remove();  
			  }  
			  else{  
			   listTemp.add(a);  
			  }  
			 }  
		    
		    return listTemp;
		}    
		
②


	/**
		 * list去重-直接将结果赋值给另一个List；
		 * @return
		 */
		public static  List<String> listRM(List<String> list) {  
		    List<String> tempList= new ArrayList<String>();  
		    for(String i:list){  
		        if(!tempList.contains(i)){  
		            tempList.add(i);  
		        }  
		    }  
		    
		    return tempList;
		}     

③

	/**
		 * list去重-用set去重
		 * @return
		 */
		public static  List<String> listRM3(List<String> list) {  
			List<String> listTemp= null;  
			HashSet<String> set = new HashSet<String>();
			set.addAll(list);
			listTemp = new ArrayList<String>(set);
		    return listTemp;
		}  
