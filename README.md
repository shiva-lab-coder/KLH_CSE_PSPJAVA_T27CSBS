PROJECT TITLE: CINEMA SEAT BOOKING SYSTEM

TEAM NUMBER: KLH/PSPJAVA/S5/T29

TEAM MEMBERS WITH ID NUMBERS:

1. 2620030194- M SHIVA SAI  
2. 2620090051-B.VNSSMSK KARTHIK

SUPERVISOR’S NAME: BALAIAH GUDIPUDI

ABSTARCT:

The Cinema Seat Booking System is a Java-based console application developed to simplify cinema ticket booking and billing operations. The application displays a list of movies along with their ticket prices and showtimes, allowing users to select a movie, specify the number of tickets, and enter their preferred seat numbers within the range of 1 to 40. It also provides a coupon-code feature that offers a discount of ₹100 when a valid coupon code is entered.

The system calculates the final booking amount based on the selected movie's ticket price, the number of seats, and the applicable discount. It generates a booking receipt containing the booking ID, movie name, showtime, selected seat numbers, ticket count, discount, and final payable amount. The application also displays GST information, stores booking receipts in a text file, generates a booking report, calculates the total collection, and sorts movies alphabetically.

This project demonstrates several Java programming concepts, including classes and objects, interfaces, the Comparable interface, exception handling, file handling, and the Java Collections Framework using List, Set, and Map. It also utilizes lambda expressions, the Stream API, the Predicate functional interface, and string handling to process booking information and perform billing operations efficiently.

The main objective of this project is to demonstrate the practical application of core Java programming concepts in developing a simple and user-friendly cinema ticket booking system. The application operates as a standalone console program without external APIs or database connectivity, using Java's built-in libraries and text-file storage to manage booking records.

PROJECT STRUCTURE:

• CODE:

```java
import java.util.*;
import java.nio.file.*;
import java.io.IOException;
import java.util.function.Predicate;
import java.util.stream.Collectors;

interface Billable                                                                             //CO4 COVERED
{
    double calculateBill();             
}

class Movie implements Comparable<Movie> 
{
    private String name;
    private double price;
    private String showTime;

    Movie(String name, double price, String showTime) 
    {
        this.name = name;
        this.price = price;
        this.showTime = showTime;
    }

    public String getName()
    {
        return name;
    }

    public double getPrice() 
    {
        return price;
    }

    public String getShowTime() 
    {
        return showTime;
    }

    @Override
    public int compareTo(Movie m) 
    {
        return name.compareToIgnoreCase(m.name);
    }

    @Override
    public String toString() 
    {
        return name + " - Rs." + price + " - " + showTime;
    }
}

class Booking implements Billable 
{
    private static int count = 0;
    private int id;
    private Movie movie;
    private List<Integer> seats;
    private double discount;

    Booking(Movie movie, List<Integer> seats, double discount) 
    {
        this.id = ++count;
        this.movie = movie;
        this.seats = new ArrayList<>(seats);
        this.discount = discount;
    }

    @Override
    public double calculateBill()                                                             //CO3 COVERED
    {
        return Math.max(0, movie.getPrice() * seats.size() - discount);
    }

    public String toString() 
    {
        return "Booking ID: " + id + "\nMovie: " + movie.getName()
            + "\nShow Time: " + movie.getShowTime()
            + "\nSeats: " + String.join(", ",
                seats.stream().map(String::valueOf)
                .collect(Collectors.toList()))
            + "\nTickets: " + seats.size()
            + "\nDiscount: Rs." + discount
            + "\nFinal Amount: Rs." + calculateBill();
    }
}

public class CinemaSeatBookingSystem 
{
    public static void main(String[] args) 
    {
        Scanner sc = new Scanner(System.in);
        List<Movie> movies = new ArrayList<>();                                               //CO6 COVERED                
        List<Booking> bookings = new ArrayList<>();
        Set<Integer> selectedSeats = new HashSet<>();
        Map<Integer, Booking> bookingMap = new HashMap<>();
        Path file = Paths.get("bookings.txt");

        movies.add(new Movie("Dune Part 3", 250, "10:00 AM"));
        movies.add(new Movie("Avengers Doomsday", 350, "1:00 PM"));
        movies.add(new Movie("The Paradise", 460, "4:00 PM"));
        movies.add(new Movie("Salaar 2", 370, "7:00 PM"));

        try {
            System.out.println("======= MOVIES =======");

            for (int i = 0; i < movies.size(); i++) 
            {
                System.out.println((i + 1) + ". " + movies.get(i));
            }

            System.out.print("Choose a movie (1-4): ");
            int choice = Integer.parseInt(sc.nextLine());

            if (choice < 1 || choice > movies.size())
                throw new IllegalArgumentException("Invalid movie choice.");

            Movie selected = movies.get(choice - 1);
            System.out.println("Selected Movie: " + selected.getName());

            System.out.print("Enter number of seats (1-40): ");
            int numberOfSeats = Integer.parseInt(sc.nextLine());

            if (numberOfSeats < 1 || numberOfSeats > 40)                                     //CO2 COVERED
            {
                throw new IllegalArgumentException("Enter 1 to 40 seats.");
            }

            System.out.println("Show Time: " + selected.getShowTime());
            System.out.print("Enter coupon code (or NONE): ");
            String coupon = sc.nextLine().trim().toUpperCase();                              //CO5 COVERED
            double discount = coupon.equals("ILOVEU143") ? 100 : 0;

            for (int i = 1; i <= numberOfSeats; i++) 
            {
                System.out.print("Enter seat number (1-40): ");
                int seat = Integer.parseInt(sc.nextLine());

                if (seat < 1 || seat > 40) 
                {
                    throw new IllegalArgumentException("Invalid seat number.");
                }

                if (!selectedSeats.add(seat)) 
                {
                    throw new IllegalArgumentException("Seat already selected.");
                }
            }

            List<Integer> seats = new ArrayList<>(selectedSeats);
            seats.sort(Integer::compareTo);

            Booking booking = new Booking(selected, seats, discount);
            bookings.add(booking);
            bookingMap.put(bookings.size(), booking);

            System.out.println("\n======= BOOKING RECEIPT =======");
            System.out.println(booking);
            System.out.println("GST: 1.4% (display only)");

            Files.writeString(file, booking + System.lineSeparator(),
                StandardOpenOption.CREATE,
                StandardOpenOption.APPEND);

            System.out.println("\n======= BOOKING REPORT =======");
            Predicate<Booking> valid = b -> b.calculateBill() >= 0;

            bookings.stream()
                .filter(valid)
                .map(Booking::toString)
                .forEach(System.out::println);

            double total = bookings.stream()
                .mapToDouble(Booking::calculateBill)
                .sum();

            System.out.println("Total Collection: Rs." + total);

            Collections.sort(movies);
            System.out.println("\nMovies sorted alphabetically:");
            movies.forEach(System.out::println);

            System.out.println("Booking saved to bookings.txt");

        } 
        catch (NumberFormatException e) 
        {
            System.out.println("Error: Enter numbers only.");                              //CO1 COVERED
        } 
        catch (IllegalArgumentException e)                                           
        {
            System.out.println("Error: " + e.getMessage());
        } 
        catch (IOException e) 
        {
            System.out.println("File error: " + e.getMessage());
        } 
        finally 
        {
            sc.close();
            System.out.println("Thank you! Visit again.");
        }
    }
}
```
