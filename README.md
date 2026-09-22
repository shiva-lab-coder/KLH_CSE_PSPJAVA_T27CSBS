PROJECT TITLE: CINEMA SEAT BOOKING SYSTEM
TEAM NUMBER: KLH/PSPJAVA/S5/T29
TEAM MEMBERS WITH ID NUMBERS:
1. 2620030194- M SHIVA SAI
2. 2620090051-B.VNSSMSK KARTHIK
 
SUPERVISOR’S NAME: BALAIAH GUDIPUDI
ABSTARCT:






PROJECT STRUCTURE:
• CODE:
import java.util.Scanner;

public class CinemaSeatBookingSystem
{
    public static void main(String[] args)
    {
        Scanner mb = new Scanner(System.in);

        
        String[] movieNames = {"Dune Part 3","Avengers Doomsday","The Paradise","Salaar 2"};      

        int[] moviePrices = {250, 350, 460, 370};

        System.out.println("===========MOVIES STREAMING NOW==========");

        for (int i = 0; i < movieNames.length; i++)                                            //CO3 COVERED 
        {
            System.out.println((i + 1) + ". " + movieNames[i] + " - price : Rs." + moviePrices[i]);
        }

        System.out.print("Enter the movie number (1-4) :");
        int moviename = mb.nextInt();

        mb.nextLine();

        System.out.print("Enter the Copoun Code : ");
        String CoupounCode = mb.nextLine();                                                    //CO1 COVERED

        int discount = 0;

        if (CoupounCode.equals("ILOVEU143"))
        {
            discount = 100;
            System.out.println("Success! Coupon applied. Rs."
                    + discount + " discount applied.");
        }
        else
        {
            System.out.println("No valid coupon applied.");
        }

        String finalMovie = "";
        int price = 0;

        switch (moviename)
        {
            case 1:
                finalMovie = movieNames[0];
                price = moviePrices[0];
                System.out.println("Selected movie : " + finalMovie);
                break;

            case 2:
                finalMovie = movieNames[1];                                                    //CO2 COVERED
                price = moviePrices[1];
                System.out.println("Selected movie : " + finalMovie);
                break;

            case 3:
                finalMovie = movieNames[2];
                price = moviePrices[2];
                System.out.println("Selected movie : " + finalMovie);
                break;

            case 4:
                finalMovie = movieNames[3];
                price = moviePrices[3];
                System.out.println("Selected movie : " + finalMovie);
                break;

            default:
                System.out.println("invalid movie name");
                
        }

        System.out.println("========== SHOWTIME =========");
        System.out.println("1. MORNING");
        System.out.println("2. EVENING");

        System.out.print("Enter the showtime: ");
        int showtime = mb.nextInt();

        switch (showtime)
        {
            case 1:
                System.out.println("Selected Showtime : MORNING");
                break;

            case 2:
                System.out.println("Selected Showtime : EVENING");
                break;

            default:
                System.out.println("Invalid Showtime");
            
        }

        System.out.print("Select seat number (1-40): ");
        int m = mb.nextInt();

        if (m > 0 && m <= 40)
        {
            System.out.println("Your booking confirmed!");
            System.out.println("Enjoy your show.\n");
        }
        else
        {
            System.out.println("Invalid seat.");
            
        }

        int finalPrice = price - discount;

        if (finalPrice < 0)
        {
            finalPrice = 0;
        }
        
        System.out.println("===========Movie Bill============");
        System.out.println("Movie name : " + finalMovie);
        System.out.println("Base Price : Rs." + price);

        if (discount > 0)
        {
            System.out.println("Discount   : Rs." + discount);
        }

        System.out.println("Total Paid : Rs." + finalPrice);
        System.out.println("GST : 1.4%");
        System.out.println("Seat no : " + m);

        System.out.println("==========Thank You Visit Again ==========");

        mb.close();
    }
}
