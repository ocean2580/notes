# int strlen(const char * str);



```c
int Len(const char *str){
	int c = 0;
	while(*(str++)){
		c ++;
	}
	return c;
}
```

# int strcmp(const char *str1,                       const char * str2)

```c
int Compare(const char *str1,const char *str2){
	while(*str1 == *str2){
		str1 ++;
		str2 ++;
		
		if((*str1 == '\0') && (*str2 == '\0')){
			return 0;
			
		}else{//某一字符串结束 
		
		if(*str1 == '\0'){
			return -1;
		}
		if(*str2 == '\0'){
			return 1;
		}
	}
        
    }
	
	//不等情况 
	if(*str1 < *str2) return -1;
	else if (*str1 > *str2) return 1;
	
}
```

# char *strcpy(char * str1,const char * str2);

```c
void Copy(char * str1,const char *str2){
	while(*str2){
		*str1 = *(str2);
		str2 ++;
		str1 ++;
	}
	*str1 = '\0';//终止
}
```

# char* strcat(char *str1,const char * str2);

```C
void Cat(char * str1,const char *str2){
	while(*str1){//指尾 
		str1 ++;
	}
	
    //Copy();
	while(*str2){
		*str1 = *(str2);
		str2 ++;
		str1 ++;
	}
	*str1 = '\0';
	
}
```

