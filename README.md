# RERehabProject
Real Estate Rehab Calculator
// Programmer:  Christopher Randall
// I am building this program to help my wife and I calculate Real Estate Rehb calculation faster and on the go.
// Eventually this will be an application for mobile devices that we use.  However, this is my first project of
// this size.  I have been working through some books and am at the point where I ma not fully understanding 
// the save and load data code that I am writing.  I need some help getting through this save and load step.
// The objective currently is to gathre user input data, conduct calculations and purchase offers, then save
// data for future usage/printing.  Loading and editing that data is the next important step after getting
// the data to save correctly. I also know my code is horrible ineffecient as this is my first real project.

#include "stdafx.h"
#include <iostream>
#include <iomanip>
#include <string>
#include <fstream>
#include <conio.h>

using namespace std;

//prototype functions
void menu(int &c), resaleValue(), buyingCosts(), sellingCosts(), carryingCosts(), repairCosts(), desiredProfits(), calculateOffer(), nameReport();
void readData(void);
string * split(string, char);

//declare variables
int choice = 0;
double estSalePrice, totBuyCost, totSellCost, totCarryCost, repairAmount, desProfAmount, investorTopOffer, investorFirstOffer;
int const SIZE = 80, MIN = 6;
const char FileName[] = "TestAddress.txt";

//declare arrays
string rehabName;
string buyingCategories[14], sellingCategories[9], carryingCategories[8];
double buyingAmounts[14], sellingAmounts[9], carryingAmounts[8];

int main()
{
	//initializing buying array
	buyingCategories[0] = "Title Work";
	buyingCategories[1] = "Recording Fees";
	buyingCategories[2] = "Apprasial";
	buyingCategories[3] = "Prepaid Interest";
	buyingCategories[4] = "Lender Fees";
	buyingCategories[5] = "Points";
	buyingCategories[6] = "Attorney Fees";
	buyingCategories[7] = "Pest Report";
	buyingCategories[8] = "Survey";
	buyingCategories[9] = "Prepaid Escrow";
	buyingCategories[10] = "Sellers Arrearges";
	buyingCategories[11] = "Inspection";
	buyingCategories[12] = "Special Insurance";
	buyingCategories[13] = "Closing Agent Fees";

	//initializing selling array
	sellingCategories[0] = "Closing Costs";
	sellingCategories[1] = "Advertising";
	sellingCategories[2] = "Internet Expenses";
	sellingCategories[3] = "Signs";
	sellingCategories[4] = "Recording Fees";
	sellingCategories[5] = "Transfer Taxes";
	sellingCategories[6] = "Attorney Fees";
	sellingCategories[7] = "Answering Service";
	sellingCategories[8] = "Real Estate Commissions";

	//initializing carrying array
	carryingCategories[0] = "Note/Investor Payments";
	carryingCategories[1] = "Property Taxes";
	carryingCategories[2] = "Insurance";
	carryingCategories[3] = "Cleaning";
	carryingCategories[4] = "HOA Fees";
	carryingCategories[5] = "Lawn and Snow";
	carryingCategories[6] = "All Utilities (Electric, Gas, etc...)";
	carryingCategories[7] = "Other";



	//call menu function
	menu(choice);

	//evaluate menu selection and trigger response
	while (choice > 0)
	{

		switch (choice)
		{

			//call various functions based on menu selection
		case 1:
			std::cout << endl;
			nameReport();
			std::cout << endl;
			resaleValue();
			std::cout << endl;
			std::cout << "                Estimated Resale Value Saved!\n";
			std::cout << endl;
			std::system("pause");
			std::system("CLS");
			break;
		case 2:
			buyingCosts();
			std::cout << endl;
			std::cout << "                Buying Costs Saved!\n";
			std::cout << endl;
			std::system("pause");
			std::system("CLS");
			break;
		case 3:
			sellingCosts();
			std::cout << endl;
			std::cout << "                Selling Costs Saved!\n";
			std::cout << endl;
			std::system("pause");
			std::system("CLS");
			break;
		case 4:
			carryingCosts();
			std::cout << endl;
			std::cout << "                Carrying Costs Saved!\n";
			std::cout << endl;
			std::system("pause");
			std::system("CLS");
			break;
		case 5:
			repairCosts();
			std::cout << endl;
			std::cout << "                Repair Costs Saved!\n";
			std::cout << endl;
			std::system("pause");
			std::system("CLS");
			break;
		case 6:
			desiredProfits();
			std::cout << endl;
			std::cout << "                Desired Profits Saved!\n";
			std::cout << endl;
			std::system("pause");
			std::system("CLS");
			break;
		case 7:
			std::system("CLS"); //created a report look to display calculateOffer cleaner
			std::cout << endl;
			std::cout << "*****************************************************************************\n";
			std::cout << "**                                                                         **\n";
			std::cout << "**                         Offer Calculation Report                        **\n";
			std::cout << "**                                                                         **\n";
			std::cout << "*****************************************************************************\n";
			std::cout << endl;
			std::cout << "\t\t\t\t" << rehabName << endl;
			std::cout << endl;
			calculateOffer();
			std::cout << endl;
			/*std::cout << "To Save report ";
			std::system("pause");
			writeData();*/
			std::cout << "To return to menu ";
			std::system("pause");
			std::system("CLS");
			break;
		case 8:
			std::cout << system("CLS");
			readData();
			std::cout << endl;
			std::cout << "\tYour Report for " << rehabName << " has been Loaded!\n";
			std::cout << endl;
			std::system("pause");
			std::system("CLS");
			break;
		default:
			break;
		}


		//set to return to menu after completing each function
		menu(choice);

	}
	//goodbye message
	std::cout << endl;
	std::cout << "                Thanks for using the Real Estate Rehab Calculator!\n";
	std::cout << endl;
	std::system("pause");

	return 0;
}

//menu function
void menu(int &c)
{
	//menu display more user friendly
	std::cout << "****************************************************************************\n";
	std::cout << "**                                                                        **\n";
	std::cout << "**              Welcome to the Real Estate Rehab Calculator!              **\n";
	std::cout << "**                                                                        **\n";
	std::cout << "****************************************************************************\n";
	std::cout << endl;
	std::cout << "                Please Select from the menu Below.\n";
	std::cout << endl;
	std::cout << "        1: Input Estimated Resale Value\n";
	std::cout << "        2: Input Buying Costs\n";
	std::cout << "        3: Input Selling Costs\n";
	std::cout << "        4: Input Carrying Costs\n";
	std::cout << "        5: Input Repair Costs\n";
	std::cout << "        6: Input Desired Profits\n";
	std::cout << "        7: Display Report and Save Report\n";
	std::cout << "        8: Load saved Report\n";
	std::cout << "        0: to Quit\n";
	std::cout << endl;
	std::cout << "                Selection: ";
	cin >> c;
	std::cout << endl;
	while (c < 0 || c > 8) //validation for user input
	{
		std::cout << "                ERROR! Please enter a number between 1 - 8 or 0." << endl;
		std::cout << endl;
		std::cout << "****************************************************************************\n";
		std::cout << "**                                                                        **\n";
		std::cout << "**              Welcome to the Real Estate Rehab Calculator!              **\n";
		std::cout << "**                                                                        **\n";
		std::cout << "****************************************************************************\n";
		std::cout << endl;
		std::cout << "                Please Select from the menu Below.\n";
		std::cout << endl;
		std::cout << "        1: Input Estimated Resale Value\n";
		std::cout << "        2: Input Buying Costs\n";
		std::cout << "        3: Input Selling Costs\n";
		std::cout << "        4: Input Carrying Costs\n";
		std::cout << "        5: Input Repair Costs\n";
		std::cout << "        6: Input Desired Profits\n";
		std::cout << "        7: Display Report and Save Report\n";
		std::cout << "        8: Load saved Report\n";
		std::cout << "        0: to Quit\n";
		std::cout << endl;
		std::cout << "                Selection: ";
		cin >> c;
		std::cout << endl;
	}
}

//estimated resale value function
void resaleValue()
{
	std::cout << "Please enter the Estimated Resale Value of the property: $";
	cin >> estSalePrice;
	while (estSalePrice < 0) //input validation
	{
		std::cout << "ERROR! Please enter a new number zero (0) or above.\n" << endl;
		std::cout << "Please enter the Estimated Resale Value of the property: $";
		cin >> estSalePrice;
	}
}

//buying costs function
void buyingCosts()
{
	std::cout << "Please enter in the amounts for the following 14 costs. Note that zero (0) can be an amount.\n";
	for (int i = 0; i < 14; i++) //display each buying category and capture the user inputted amounts
	{
		std::cout << "Enter amount for " << buyingCategories[i] << " : $";
		cin >> buyingAmounts[i];
		while (buyingAmounts[i] < 0) //input validation
		{
			std::cout << "ERROR! Please enter a new number zero (0) or above.\n" << endl;
			std::cout << "Enter amount for " << buyingCategories[i] << " : ";
			cin >> buyingAmounts[i];
		}
		totBuyCost += buyingAmounts[i]; //calculates total buying costs and stores for later usage
	}
}

//selling costs function
void sellingCosts()
{
	std::cout << "Please enter in the amounts for the following 9 costs. Note that zero (0) can be an amount.\n";
	for (int i = 0; i < 9; i++) //display each selling category and capture the user inputted amounts
	{
		std::cout << "Enter amount for " << sellingCategories[i] << " : $";
		cin >> sellingAmounts[i];
		while (sellingAmounts[i] < 0) //input validation
		{
			std::cout << "ERROR! Please enter a new number zero (0) or above.\n" << endl;
			std::cout << "Enter amount for " << sellingCategories[i] << " : ";
			cin >> sellingAmounts[i];
		}
		totSellCost += sellingAmounts[i]; //calculates total selling costs and stores for later usage
	}
}

//caryying costs function
void carryingCosts()
{
	std::cout << "Please enter in the amounts for the following 8 costs. Note that zero (0) can be an amount.\n";
	for (int i = 0; i < 8; i++) //display each carrying category and capture the user inputted amounts
	{
		std::cout << "Enter amount for " << carryingCategories[i] << " : $";
		cin >> carryingAmounts[i];
		while (carryingAmounts[i] < 0) //input validation
		{
			std::cout << "ERROR! Please enter a new number zero (0) or above.\n" << endl;
			std::cout << "Enter amount for " << carryingCategories[i] << " : ";
			cin >> carryingAmounts[i];
		}
		totCarryCost += carryingAmounts[i]; //calculates total carrying costs and stores for later usage
	}
}

//repair costs function
void repairCosts()
{
	std::cout << "Please enter the Total Repair Costs for the property: $";
	cin >> repairAmount;
	while (repairAmount < 0) //input validation
	{
		std::cout << "ERROR! Please enter a new number zero (0) or above.\n" << endl;
		std::cout << "Please enter the Total Repair Costs for the property: $";
		cin >> repairAmount;
	}
}

//desired profits function
void desiredProfits()
{
	std::cout << "Please enter your Desired Profits from the Sale of the property: $";
	cin >> desProfAmount;
	while (desProfAmount < 0) //input validation
	{
		std::cout << "ERROR! Please enter a new number zero (0) or above.\n" << endl;
		std::cout << "Please enter your Desired Profits from the Sale of the property: $";
		cin >> desProfAmount;
	}
}

//offer calculation function
void calculateOffer()
{
	//overall input calculations
	investorTopOffer = estSalePrice - totBuyCost - totSellCost - totCarryCost - repairAmount - desProfAmount;
	investorFirstOffer = (investorTopOffer * .20);
	investorFirstOffer = investorTopOffer - investorFirstOffer;

	//display format setup using \t where I was using just spaces before
	std::cout << "\t\tEstimated Sale Price:\t\t\t$" << fixed << setprecision(2) << estSalePrice << "\n" << endl; //set precision to hold .00 decimals
	std::cout << "\t\tTotal Buying Costs:\t\t\t$" << totBuyCost << "\n" << endl;
	std::cout << "\t\tTotal Selling Costs:\t\t\t$" << totSellCost << "\n" << endl;
	std::cout << "\t\tTotal Carrying Costs:\t\t\t$" << totCarryCost << "\n" << endl;
	std::cout << "\t\tTotal Repair Costs:\t\t\t$" << repairAmount << "\n" << endl;
	std::cout << "\t\tDesired Profits from Sale:\t\t$" << desProfAmount << "\n" << endl;
	std::cout << endl;
	std::cout << "_____________________________________________________________________________\n";
	std::cout << "    Suggested First Offer for this property is\t\t$" << investorFirstOffer << "\n";
	std::cout << "    (20% less then top offer.)\n" << endl;
	std::cout << "    Your Top Offer for this property is\t\t\t$" << investorTopOffer << "\n" << endl;

	std::cout << "To Save report ";
	std::system("pause");

	//writing data function built into display report menu selection
		
	fstream outputFile;
	outputFile.open(FileName, fstream::app); //opens file in append mode

	//writes all data to the file
	outputFile << estSalePrice << "#" << totBuyCost << "#" << totSellCost << "#" << totCarryCost << "#" << repairAmount << "#" << desProfAmount << "#" << endl;

	//Close File
	outputFile.close();
	std::cout << endl;
	std::cout << "Report has been Saved";
	std::cout << endl;
}


void nameReport()
{
	std::cout << endl;
	cout << "Please enter the Name for this ReHab: ";
	cin >> rehabName;

}

void readData(void)
{
	ifstream inMyStream(FileName);

	if (inMyStream.is_open())
	{

		string recBreaks = "";
		recBreaks.assign(20, '-');
		int fieldCount = 0; //keep track of the number of fields read
		int recordCount = 1; //keep track of the number of records read

		//read the first field
		fieldCount = 1;
		string fieldBuffer;
		getline(inMyStream, fieldBuffer);

		while (!inMyStream.eof())
		{

			int index = fieldBuffer.find("\n");

			if (index != -1)
				fieldBuffer.erase(index, 1);

			switch (fieldCount)
			{
			case 1:
				cout << recBreaks << endl;
				cout << "Record #" << recordCount << endl;
				cout << "Estimated Sale Price " << fieldBuffer << endl;
				break;
			case 2:
				cout << "Total Buying Costs " << fieldBuffer << endl;
				break;
			case 3:
				cout << "Total Selling Costs " << fieldBuffer << endl;
				break;
			case 4:
				cout << "Total Carrying Costs " << fieldBuffer << endl;
				break;
			case 5:
				cout << "Total Repair Costs " << fieldBuffer << endl;
				break;
			case 6:
				cout << "Total Desired Profits " << fieldBuffer << endl;
				fieldCount = 0;
				recordCount++;
				break;
			}
			//read the next field
			getline(inMyStream, fieldBuffer);
			fieldCount++;
		}
		cout << recBreaks << endl;
		inMyStream.close();
	}//end read data

}

string * split(string theLine, char theDeliminator)
{
	int splitCount = 0;

	for (int i = 0; i < theLine.size(); i++)
	{

		if (theLine[i] == theDeliminator)
			splitCount++;
	}

	splitCount++;

	string* theFieldArray;
	theFieldArray = new string[splitCount];
	string theField = "";
	int commaCount = 0;

	for (int i = 0; i < theLine.size(); i++)
	{

		if (theLine[i] != theDeliminator)
		{
			theField += theLine[i];
		}
		else
		{
			theFieldArray[commaCount] = theField;
			theField = "";
			commaCount++;
		}
	}
	theFieldArray[commaCount] = theField;

	return theFieldArray;
}//end split


