# Segmentation
Display Segment Table &amp; Physical Memory Initialize p0: 8, p1:10, p2: 14, p3: 4 Create segments and allocate memory to each processes Values of these Blocks will be process number Display the states of arrays after allocation Remove process p0 &amp; P2 Display array states Add a new process P4: 12 Allocate the Segment &amp; Memory Space Get all the Blocks of memory that are allocated to p1



#include<iostream>
#include"stack.h"
using namespace std;

int  Process[20], countStorage = 0,Proces_size=0,limit,base;
int PhysicalStorage[40];


class MemoryManager
{
public:
	SegmentTable segment_table;

	void DisplayContent();
	void Initialize();
	void initializeMemory();
	void insertSegmet(int index);
	void checkMemory(int size);
	void SegmnetTable(int ProcessNo);
	void deleteProcess(int ProcessNo);
	void MMU(int PocessNo);
};

void MemoryManager:: DisplayContent()
{
	cout << "\t\tMain memory is :: \n";
	cout << "\tindex\tStored_Data\n\n";
	for (int i = 0; i <= 40; i++)
	{
		cout << "\t" << i << "\t" << PhysicalStorage[i] << endl;
	}
	cout << "\t\tSegment table  is :: \n";
	cout << "SegmentNo\tBaseNo\tLimitNo\n\n";
	segment_table.traverse();

}
void MemoryManager::Initialize()
{
	for (int i = 0; i <= 40; i++)
	{

		PhysicalStorage[i] = -1;

	}

	for (int i = 0; i < 10; i++)
	{
		segment_table.IAE(i, -1, -1);

	}


}
void MemoryManager::initializeMemory()
{
	Process[0] = 8;
	Process[1] = 10;
	Process[2] = 6;
	Process[3] = 4;

	insertSegmet(0);
	insertSegmet(1);
	insertSegmet(2);
	insertSegmet(3);
	Proces_size = 3;

}

void MemoryManager:: checkMemory(int size)
{
	countStorage = 0;
	int count=0;

	for (int i = 0; i <= 40 ; i++)
	{
		int j = i;
		if (PhysicalStorage[i] == -1)
		{
			while (count != size)
			{
				if (PhysicalStorage[j] == -1)
				{
					count++;
					j++;
					
				}
				if (count == size - 1)
				{
					
					base = i;
					limit = j;
					countStorage = i;
					break;
				}
			}
		}
		if (count == size - 1)
		{
			break;
		}
		
	}

}

void MemoryManager:: insertSegmet(int index)
{

	int  processNo = index;
	int value = Process[index];


	checkMemory(value);

	for (int i = 0; i < value; i++)
	{
		
		PhysicalStorage[countStorage] = processNo;
		countStorage++;
	}

	SegmnetTable(processNo);
}
void MemoryManager::SegmnetTable(int ProcessNo)
{
	segment_table.Insert(ProcessNo, base, limit);
}
void MemoryManager::deleteProcess(int ProcessNo)
{
	int check = ProcessNo;
	for (int i = 0; i <= 45; i++)
	{
		if (PhysicalStorage[i] == check)
		{
			PhysicalStorage[i] = -1;
		}
	}

	segment_table.Insert(ProcessNo, -1,-1);

}


void MemoryManager::MMU(int ProcessNo)
{

	segment_table.getData(ProcessNo, base, limit); // getting data from the table

	cout << "index\tSegmentNo\n\n";
	for (int i = base; i <=limit ; i++)
	{
		cout << i <<"\t"<< PhysicalStorage[i] << endl;;
	}

	
}








void main()
{

	int num_proc,count=0,choice, ProcessNo,size;
	MemoryManager M;
	
	M.Initialize();
	M.DisplayContent();
	M.initializeMemory();
	M.DisplayContent();



	do
	{

		cout << "\n\tPress 1 to Add Segment to Memory";
		cout << "\n\tPress 2 to Remove Segment from Memory";
		cout << "\n\tPress 3 to Retrieve Data of Segment";
		cout << "\n\tPress 4 to Display Memory\n";
		cout << "\n\t\t Enter :: ";
		cin >> choice;

	
		if (choice == 1)
		{
			cout << "\n\tEnter Segment Size to Add :: ";
			cin >> size;
			Proces_size++;
			Process[Proces_size] = size;
			M.insertSegmet(Proces_size);
		}
		if (choice == 2)
		{
			cout << "\n\tEnter Segment No to delete :: ";
			cin >> ProcessNo;
			M.deleteProcess(ProcessNo);
		}
		if (choice == 3)
		{
			cout << "\n\tEnter Segment No to find :: ";
			cin >> ProcessNo;
			M.MMU(ProcessNo);
		}
		if (choice == 4)
		{
			M.DisplayContent();
		}

		cout << "\n\tPress 1 to try agin and 2 to exit :: ";
		cin >> choice;

	} while (choice==1);

}

