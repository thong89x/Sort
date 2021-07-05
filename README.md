#include<iostream>
#include<string>
#include<ctime>
#include "DataGenerator.h"
#include"sort.h"
#include<sstream>
#include<fstream>

using namespace std;




void Sort(int *a, int n, string sort,long long&count_compare,double&time)
{
	time=0;
	if(sort=="insertion-sort") insertionSort(a,n,count_compare,time);
	else  if(sort=="heap-sort") heapSort(a,n,count_compare,time);
	else  if(sort=="shaker-sort") shakerSort(a,n,count_compare,time);	
	/*if(sort=="bubble-sort") bubblesort(a,n,count_compare,time);
	else if(sort=="selection-sort") selection(a,n);
	
	else if(sort=="insertion-sort") insertion(a,n,count_compare,);
	else if(sort=="heap-sort") heap-Sort(a,n);
	else if(sort=="quick-sort") quicksort(a,n);
	else if(sort=="merge-sort") 
	{
		clock_t start=clock();
		mergesort(a,n);	
		clock_t end=clock();
		time=double(end-start)/CLOCKS_PER_SEC;
	}
	else if(sort=="radix-sort") radixsort(a,n);
	else if(sort=="flash-sort") flashsort(a,n);
	else if(sort=="counting-sort") countingsort(a,n);
	else if(sort=="shell-sort") shellsort(a,n);*/
}
void Output_parameters(string&s,long long count_compare,double time)
{
	if(s=="-comp")	cout<<"Comparitions : "<<count_compare<<endl;
	else if(s=="-time") cout<<"Running time : "<<time<<endl;
	else
	{
		cout<<"Comparitions : "<<count_compare<<endl;
		cout<<"Running time : "<<time<<endl;
	}

}
void Command_1(string* str)
{
	long long count_compare=0,count_assign=0;
	double time=0;
	ifstream in(str[3]);
	int n;
	in>>n;
	int *arr= new int [n];
	int i=0;
	while(!in.eof())
	{
		in>>arr[i++];
	}
	in.close();
	Sort(arr,n,str[2],count_compare,time);
	ofstream out("output.txt");
	for(int j=0;j<n;j++)
		out<<arr[j]<<" ";
	out<<endl;
	out.close();
	cout<<"ALGORITHM MODE"<<endl;
	cout<<"Algorithm: "<<str[2]<<endl;
	cout<<"Input file: "<<str[3]<<endl;
	cout<<"Input size: "<<n<<endl;
	cout<<"-------------------------------------"<<endl;
	cout<<"cuoi"<<endl;
	Output_parameters(str[4],count_compare,time);
	delete []arr;
}
void Command_2(string* str)
{
	long long count_compare=0,count_assign=0;
	int n;
	stringstream ss(str[3]);
	ss>>n;
	cout<<"ALGORITHM MODE"<<endl;
	cout<<"Algorithm: "<<str[2]<<endl;
	cout<<"Input size: "<<n<<endl;	
	string order[]={"Randommize","Sorted","Reversion","Nearly Sorted"};
	int mode=-1;
	if(str[4]=="-rand") mode=0;
	else if(str[4]=="-sorted") mode=1;
	else if(str[4]=="-rev") mode=2;
	else if(str[4]=="-nsorted") mode=3;
	cout<<mode<<endl;
	int *arr= new int [n];
	count_compare=0;
	double time=0;
	GenerateData(arr,n,mode);
	
	Sort(arr,n,str[2],count_compare,time);
	
	ofstream out("output.txt",ios::out);	
	out<<n<<endl;
	for(int j=0;j<n;j++)
		out<<arr[j]<<" ";
	out<<endl;
	out.close();
	cout<<endl<<"Input order: "<<order[mode]<<endl;
	cout<<"-------------------------------------"<<endl;
	Output_parameters(str[5],count_compare,time);
	delete []arr;		
}
void Command_3(string* str)
{	
	long long count_compare=0,count_assign=0;
	int n;
	stringstream ss(str[3]);
	ss>>n;
	cout<<"ALGORITHM MODE"<<endl;
	cout<<"Algorithm: "<<str[2]<<endl;
	cout<<"Input size: "<<n<<endl;	
	string filename[]={"input_1.txt","input_2.txt","input_3.txt","input_4.txt"};
	string order[]={"Randommize","Sorted","Reversion","Nearly Sorted"};
	for(int i=0;i<4;i++)
	{

		int *arr= new int [n];
		count_compare=0;
		double time=0;
		GenerateData(arr,n,i);
		ofstream out(filename[i],ios::out);	
		out<<n<<endl;
		for(int j=0;j<n;j++)
			out<<arr[j]<<" ";
		out<<endl;
		out.close();
		Sort(arr,n,str[2],count_compare,time);
		cout<<endl<<"Input order: "<<order[i]<<endl;
		cout<<"-------------------------------------"<<endl;
		Output_parameters(str[4],count_compare,time);
		delete []arr;		
	}
}

int main(int argc,char* argv[])
{
	string* str=new string[argc];
	for(int i=0;i<argc;i++)	str[i].assign(argv[i]);
	if(argc==5)
	{
		if(str[1]=="-a")
		{
			bool check_doc=false;
			for(int i=0;i<str[3].length();i++)
			{
				if(str[3][i]=='.')
				{
					check_doc=true;
					break;
				}			
			}
			if(!check_doc)
			{
				Command_3(str);
			}	
			else 
				Command_1(str);
		}
		//else
			//Command_4(str);	
	}
	else
	{
		if(str[1]=="-a")
		{
			Command_2(str);
		}
			
		//else
			//Command_5;
	}
}
