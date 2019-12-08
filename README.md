# Fast-Food-Billing-System
Using C Programming language to code a fast food billing system

/*
************************************
Project Name : Goh Kok Dong.c
Written By : Goh Kok Dong
Date : 21-7-2018
Purpose : Fast Food Billing System
************************************
*/

#include <stdio.h>
#include <stdlib.h>
#include <ctype.h>

// Define Constant
#define PRICE_COMBO_A 10.00
#define PRICE_COMBO_B 15.00
#define PRICE_COMBO_C 18.00
#define PRICE_COMBO_D 24.00
#pragma warning (disable:4996)

// Declaration for called function
void logo();
void menu();

void main() {
	// Declaration for int, char and double
	int custNo, qty, qtyA, qtyB, qtyC, qtyD, totalQty;
	char combo, nextCust, plasticBag, payByCard, isMember, responseForTa;
	double comboPrice, totalComboPrice, totalComboCharges, sstCharges, totalPay, amtPaid, change, discount, totalDiscount;
	double salesAmtA, salesAmtB, salesAmtC, salesAmtD, totalSales, totalSst, totalRM, totalBank, income, plasticCharges;
	double totalPlasticCharges;

	// Display logo
	logo();

	// To Define All the Variables have value
	custNo = 0;
	totalComboCharges = 0;
	qtyA = 0;
	qtyB = 0;
	qtyC = 0;
	qtyD = 0;
	salesAmtA = 0;
	salesAmtB = 0;
	salesAmtC = 0;
	salesAmtD = 0;
	totalSst = 0;
	totalRM = 0;
	totalBank = 0;
	income = 0;
	discount = 0;
	plasticCharges = 0;
	totalDiscount = 0;
	totalPlasticCharges = 0;

	do {
		// Display the menu for every customer
		menu();
		// Display the number of customer
		printf("Customer No. = %d\n", ++custNo);

		do {
			rewind(stdin);
			// Read the combo selected by customer
			printf("Combo A, B, C, D (other = exit) : ");
			scanf("%c", &combo);
			combo = toupper(combo);

			// To confirm the customer wants to buy or not
			// Read the quantity
			if (combo == 'A' || combo == 'B' || combo == 'C' || combo == 'D') {
				printf("Quantity : ");
				scanf("%d", &qty);

				// To define the price of combo
				switch (combo) {
				case 'A':
					comboPrice = PRICE_COMBO_A;
					break;
				case 'B':
					comboPrice = PRICE_COMBO_B;
					break;
				case 'C':
					comboPrice = PRICE_COMBO_C;
					break;
				case 'D':
					comboPrice = PRICE_COMBO_D;
					break;
				default:
					comboPrice = 0;

				}

				// Calculate the total price for quantity of combo that customer order
				totalComboPrice = qty * comboPrice;
				printf("\tCOMBO %c : %6d @ RM %.0lf = RM %9.2lf\n", combo, qty, comboPrice, totalComboPrice);

				// Apply values for daily sales report
				totalComboCharges += totalComboPrice;

				// Calculate the total quantity of each combo sold
				// Calculate the total sales of each combo sold
				switch (combo) {
				case 'A':
					qtyA += qty;
					salesAmtA += totalComboPrice;
					break;
				case 'B':
					qtyB += qty;
					salesAmtB += totalComboPrice;
					break;
				case 'C':
					qtyC += qty;
					salesAmtC += totalComboPrice;
					break;
				case 'D':
					qtyD += qty;
					salesAmtD += totalComboPrice;
					break;
				}
			}
		} while (combo == 'A' || combo == 'B' || combo == 'C' || combo == 'D');

		// Calculate SST charges
		sstCharges = totalComboCharges * 0.1;

		// Member card discount
		// Ask the customer whether he/she has member card
		rewind(stdin);
		printf("Do you have member card (Y = yes): ");
		scanf("%c", &isMember);
		isMember = toupper(isMember);

		// Discount 5%
		if (isMember == 'Y') {
			// Calculate the discount given to the customer
			discount = (totalComboCharges + sstCharges) * 0.05;
		}

		// Pay with Credit Card
		rewind(stdin);
		printf("Pay with Credit Card (Y = yes): ");
		scanf("%c", &payByCard);
		payByCard = toupper(payByCard);

		// Ask customer having here or take away
		rewind(stdin);
		printf("Take away (Y = yes): ");
		scanf("%c", &responseForTa);
		responseForTa = toupper(responseForTa);

		if (responseForTa == 'Y') {
			// Plastic Bag Charges
			// Ask customer whether need a plastic bag or not
			rewind(stdin);
			printf("Do you need Plastic Bag (Y = yes): ");
			scanf("%c", &plasticBag);
			plasticBag = toupper(plasticBag);
			if (plasticBag == 'Y')
				plasticCharges = 0.2;
		}

		// Calculate the Total of Plastic Bag Charges
		totalPlasticCharges += plasticCharges;
		// Calculate the Total Amount to pay by the customer
		totalPay = totalComboCharges + sstCharges + plasticCharges - discount;
		// Calculate the Total of discount given to customer
		totalDiscount += discount;

		// Display total Charges for customer
		printf("%27s %9s %9.2lf\n", "Combo Charges", "= RM", totalComboCharges);
		printf("%25s %11s %9.2lf\n", "Add 10% SST", "= RM", sstCharges);
		printf("%28s %8s %9.2lf\n", "Discount Given", "= RM", discount);
		printf("%32s %s %9.2lf\n", "Plastic Bag Charge", "= RM", plasticCharges);
		printf("%26s %10s %9.2lf\n", "TOTAL TO PAY", "= RM", totalPay);
		// Ask the customer to pay again if there is insufficient amount paid
		do {
			// Input the amount pay by the customer
			printf("%26s %10s    ", "Ammount Paid", "= RM");
			scanf("%lf", &amtPaid);

			if (amtPaid < totalPay)
				printf("Insufficient Amount Paid. Please try again.\n");
		} while (amtPaid < totalPay);

		// Calculate the change for customer
		change = amtPaid - totalPay;
		// Calculate the total of SST Charges
		totalSst += sstCharges;

		if (payByCard == 'Y')
			// Calculate the total of RM collected
			totalBank += totalPay;
		else
			// Calculate the total of Bank Transactions
			totalRM += totalPay;

		// Display the change due
		printf("%24s %12s %9.2lf\n\n", "Change Due", "= RM", change);
		printf("Thank You, Have A Nice Day. Please Come Again.\n\n");

		// Ask user wants to continue with the next customer or not
		rewind(stdin);
		printf("Next Customer (Y = yes): ");
		scanf("%c", &nextCust);
		nextCust = toupper(nextCust);
		printf("\n");

		// To reset the values to 0, so that next customer can purchase food
		totalComboPrice = 0.00;
		totalComboCharges = 0.00;
		sstCharges = 0.00;
		totalPay = 0.00;
		amtPaid = 0.00;
		change = 0.00;
		discount = 0.00;
		plasticCharges = 0.00;

	} while (nextCust == 'Y');

	// Calculate the total quantity of combo sold for the end of the day
	totalQty = qtyA + qtyB + qtyC + qtyD;
	// Calculate the total sales for the end of the day
	totalSales = salesAmtA + salesAmtB + salesAmtC + salesAmtD;
	// Calculate the income for the end of the day
	income = totalBank + totalRM;

	// Display the Sales Report for the end of the day
	printf("\n\n");
	printf("DAILY SALES REPORT\n\n");
	// Display how many customer buy at the end of the day
	printf("Total Number of Customer = %d\n\n", custNo);
	// Display summary of sales amount for each combo for the end of the day
	printf("Combo Sales For Today\n");
	printf("%5s   %20s   %12s   \n", "Combo", "Quantity Sold", "Sales Amount");
	printf("%3s   %15d   %13.2lf\n", "A", qtyA, salesAmtA);
	printf("%3s   %15d   %13.2lf\n", "B", qtyB, salesAmtB);
	printf("%3s   %15d   %13.2lf\n", "C", qtyC, salesAmtC);
	printf("%3s   %15d   %13.2lf\n", "D", qtyD, salesAmtD);
	printf("======           ====       =========\n");
	printf("%6s   %12d   %13.2lf\n\n", "TOTALS", totalQty, totalSales);
	// Display Total of SST Charges
	printf("%23s = %11.2lf\n", "TOTAL SST charges", totalSst);
	// Display Total of discount allowed to customer
	printf("%23s = %11.2lf\n", "TOTAL Discount allowed", totalDiscount);
	// Display Total Charges for plastic bags
	printf("%23s = %11.2lf\n", "Plastic Charges", totalPlasticCharges);
	// Display Total of cash in the cash drawer
	printf("%23s = %11.2lf\n", "TOTAL RM collected", totalRM);
	// Display Total of money received from customer pay with credit card
	printf("%23s = %11.2lf\n", "TOTAL Bank Transactions", totalBank);
	// Display Total income for the end of the day
	printf("%23s = %11.2lf\n\n", "Income", income);
	
	// Greeting
	printf("Thank you for using this system\n");
	printf("=================================\n");
	printf("Written By : Goh Kok Dong\n");
	printf("=================================\n\n");

	// End main function
	system("pause");
}

void logo() {
	printf(" ______   ______   ______   ______       ___   ___   ______   ________   __   __   ______   ___   __       \n");
	printf("/_____/\\ /_____/\\ /_____/\\ /_____/\\     /__/\\ ");
	printf("/__/\\ /_____/\\ /_______/\\ /_/\\ /_/\\ /_____/\\ /__/\\ /__/\\     \n");
	printf("\\::::_\\/_\\:::_ \\ \\\\:::_ \\ \\\\:::_ \\ \\    \\::\\ \\\\  \\ \\\\::::_");
	printf("\\/_\\::: _  \\ \\\\:\\ \\\\ \\ \\\\::::_\\/_\\::\\_\\\\  \\ \\    \n");
	printf(" \\:\\/___/\\\\:\\ \\ \\ \\\\:\\ \\ \\ \\\\:\\ \\ \\ \\    \\::\\/_\\ .\\ \\\\");
	printf(":\\/___/\\\\::(_)  \\ \\\\:\\ \\\\ \\ \\\\:\\/___/\\\\:. `-\\  \\ \\   \n");
	printf("  \\:::._\\/ \\:\\ \\ \\ \\\\:\\ \\ \\ \\\\:\\ \\ \\ \\    \\:: ___::\\ \\\\");
	printf("::___\\/_\\:: __  \\ \\\\:\\_/.:\\ \\\\::___\\/_\\:. _    \\ \\  \n");
	printf("   \\:\\ \\    \\:\\_\\ \\ \\\\:\\_\\ \\ \\\\:\\/.:| |    \\: \\ \\\\:");
	printf(":\\ \\\\:\\____/\\\\:.\\ \\  \\ \\\\ ..::/ / \\:\\____/\\\\. \\`-\\  \\ \\ \n");
	printf("    \\_\\/     \\_____\\/ \\_____\\/ \\____/_/     \\__\\/ \\::\\/ \\_____\\/ ");
	printf("\\__\\/\\__\\/ \\___/_(   \\_____\\/ \\__\\/ \\__\\/ \n");
	printf("\n");
}

void menu() {
	// Shop Name
	printf("<Welcome to Food Heaven>\n\n");
	// Display Menu
	printf("\t < MENU PRICE >\n\n");
	printf("======================================================================================================\n");
	printf("Combo A :\n");
	printf("Garlic Bread with Cheese + Special Homemade Tomato Spaghetti + soft drink.\n");
	printf("Cost = RM 10.00\n");
	printf("Combo B :\n");
	printf("Grilled Chicken Chop + French Fries + Garlic Bread + soft drink.\n");
	printf("Cost = RM 15.00\n");
	printf("Combo C :\n");
	printf("Crispy Chicken bites + French Fries + Fresh skaw + soft drink.\n");
	printf("Cost = RM 18.00\n");
	printf("Combo D :\n");
	printf("Tempura Hand Battered Cod + Chargrilled Barbecue Chicken Wings + French Fries + soft drink.\n");
	printf("Cost = RM 24.00\n");
	printf("======================================================================================================\n");
	printf("Choice for drinks: \n");
	printf("1. Cocacola\n");
	printf("2. Fanta Grape\n");
	printf("3. Sprite\n");
	printf("4. Tea BOH\n");
	printf("5. Black Coffee\n");
	printf("6. White Coffee\n");
	printf("======================================================================================================\n");
	printf("Plastic Bag:\n");
	printf("Cost = RM 0.20\n");
	printf("======================================================================================================\n\n");
}
