# Meal-ordering-system
// Julia Mosaad
// Professor Marika Apostolova
// CSE 1320 002
// 04/ 20/ 2025
// CSE Final project


#include <stdio.h>
#include <string.h>
#include <stdlib.h>

#define MAX_MEALS 4
#define FILE_NAME "Meals.txt"

struct meal {
	char name[50];
	int price;
	int Quantity;
};

struct meal meals[]= {
	{"khosary", 17, 0},
	{"PshamelPasta", 15, 0},
	{"GrilledChicken", 20, 0},
	{"kofta", 10,0}
};

int calarios[MAX_MEALS] = {500, 800, 240, 450};

void chooseDish();
void enterQuantity();
void showIngredients();
void showCalories();
void showPrices();
void newOrder();
void writeToFile();
void showRecipt();


int main()
{
    int i;
	int choice;

	printf("                    Julia's cook \n ");

	printf("*******************************************************************\n");
	printf("                    Main Menu \n");

	while(1)
	{
		printf("1- Choose you Dish \n ");
		printf("2- Enter your Quantity\n");
		printf("3- See Ingredentis for your Dish\n");
		printf("4- How many calarios in your dish \n");
		printf("5- Prices\n");
		printf("6- Exit\n");
		printf("7- order a nwe order\n");
		printf("8- Save order to File\n");
		printf("9- Show Receipt\n");
		
		printf("\nEnter your choice: ");
		scanf("%d", &choice);
		
		
		switch (choice) {
		case 1:
			chooseDish();
			break;
		case 2:
			enterQuantity();
			break;
		case 3:
			showIngredients();
			break;
		case 4:
			showCalories();
			break;
		case 5:
			showPrices();
			break;
		case 6:
			printf("Exiting... Thank you!\n");
			exit(0);
		case 7:
			newOrder();
			break;
			case 8:
			writeToFile();
			break;
			
			case 9:
			showRecipt();
			break;
		default:
			printf("Invalid choice. Please try again.\n");
		}
	}
	return 0;

}


void chooseDish()
{
    
	printf("Avaliable Dishes : \n");
	for (int i = 0; i< MAX_MEALS; i++) {
		printf("%d-%s\n", i + 1, meals[i].name);
	}
}
void enterQuantity()
{

	int dish, qty;
	printf(" Enter Dish Number :\n");
	scanf("%d", &dish);

	printf("Enter Quantity: ");
	scanf("%d", &qty);
	// bc in c number starts at 0 then we are doing this code to start from 1
	// .Quantity this is to access Quantity
	// = qty which user gonna chose
	meals[dish - 1].Quantity = qty;

	printf(" you ordered %s x %d\n", meals[dish - 1].name, qty);
	
	writeToFile();
}

void showIngredients() {
	int dish;
	
	printf("Enter dish Number from (1-4 )");
	scanf("%d", &dish);

	switch(dish)
	{
	case 1:
		printf("Ingredentis for khosary : Rice, Pasta, Fried Onino, tomato sauce, koshary secret sauce, chilly\n ");
		break;

	case 2 :
		printf("Ingredentis for PshamelPasta : Pasta, meat , pashamel sauce, Extra Topings (eage or cheese)\n");
		break;

	case 3:
		printf("Ingredentis for GrilledChicken : chicken dipped into spices (chilly, mixed spices, Onino powder, Galrlic powder\n");
		break;

	case 4:
		printf("Ingredentis for kofta: meat mixed with onine and some nice flavor\n");
		break;

	default:
		printf("Invalide dish number\n");

	}

}

	void showCalories()
	{
		int dish;
		
		printf("Enter dish Number from (1-4 )");
		scanf("%d", &dish);


		if (dish < 1 || dish > MAX_MEALS) {

			printf("invalide dish number \n");
			return;
		}
		

		for (int i = 0; i < MAX_MEALS; i++)
		{
		    
			if(i == dish - 1 ) {
			    
				printf(" calarios in %s is : %d\n", meals[i].name, calarios[i]);
			}
		}
	}

 void showPrices()  
 {
        printf("\n--- Meal Prices ---\n");
        
        for (int i = 0; i < MAX_MEALS; i++) {
            
            
            printf("%d - %s: $%d\n", i + 1, meals[i].name, meals[i].price);
        }
    }
    
void newOrder ()
{
    for(int i = 0 ; i < MAX_MEALS; i++){
        meals[i].Quantity =0;
    }
    printf("New Order\n");
    
}


void writeToFile(){
    FILE *fptr = fopen("Meals.txt", "w");
    if (fptr == NULL){
        printf("The file Not exist\n");
        
        return;
        
    }
    
    fprintf(fptr, "----------------- Order Receipt ------------------\n ");
    
    for(int i = 0; i <MAX_MEALS ; i++){
        
        if (meals[i].Quantity > 0)
        
        fprintf(fptr, "%sx%d = %d\n", meals[i].name, meals[i].Quantity, meals[i].Quantity * meals[i].price);
        
        
    }

fprintf(fptr,"----------------------------------------------------------------\n");

fclose(fptr);
printf(" Order has been saved to %s. Thank you for ordering! \n", FILE_NAME);

}

void showRecipt()
{
    int total = 0;
   float tipPercent, tipAmount, finalTotal;
    
    printf(" --------- Order Summary -------------\n");
    
    for (int i = 0 ; i < MAX_MEALS ; i++){
        
        if (meals[i].Quantity > 0){
            
            int subtotal = meals[i].Quantity * meals[i].price;
            
            printf("%s x %d = $%d\n", meals[i].name, meals[i].Quantity, subtotal);
            
            total += subtotal;
        }
        
    }
    
    printf("Total: $%d\n", total);
    printf(" Enter Tip Persentage \n");

    scanf("%f", &tipPercent);
    
   tipAmount = (tipPercent / 100.0) * total;
   finalTotal = total + tipAmount;
   
   printf("tip : %.2f\n", tipAmount);
   printf(" Toal with tip : %.2f\n", finalTotal);
}
