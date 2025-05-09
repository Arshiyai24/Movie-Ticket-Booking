import java.util.*;

class User {
    String username, password;
    List<String> bookingHistory = new ArrayList<>();

    public User(String username, String password) {
        this.username = username;
        this.password = password;
    }

    public boolean login(String u, String p) {
        return username.equals(u) && password.equals(p);
    }

    public void addBooking(String details) {
        bookingHistory.add(details);
    }

    public void viewBookings() {
        System.out.println("Your Booking History:");
        for (String record : bookingHistory) {
            System.out.println("- " + record);
        }
    }
}

class Movie {
    String title;
    String language; // Local language (e.g., Tamil, Telugu, Hindi)
    List<String> showtimes;
    int availableSeats;
    double price;

    public Movie(String title, String language, List<String> showtimes, int seats, double price) {
        this.title = title;
        this.language = language;
        this.showtimes = showtimes;
        this.availableSeats = seats;
        this.price = price;
    }
}

public class MovieTicketBookingSystem {
    static Scanner sc = new Scanner(System.in);
    static List<User> users = new ArrayList<>();
    static List<Movie> movies = new ArrayList<>();
    static User currentUser = null;

    public static void main(String[] args) {
        // Prepopulate with more movies in Band India (example)
        movies.add(new Movie("விக்ரம்", "Tamil", Arrays.asList("10:00 AM", "2:00 PM", "6:00 PM"), 50, 150));
        movies.add(new Movie("RRR", "Telugu", Arrays.asList("11:00 AM", "3:00 PM", "7:00 PM"), 50, 200)); 
        movies.add(new Movie("KGF: Chapter 2", "Hindi", Arrays.asList("12:00 PM", "4:00 PM", "8:00 PM"), 50, 250)); 
        movies.add(new Movie("Thuppakki", "Tamil", Arrays.asList("10:00 AM", "2:00 PM", "6:00 PM"), 100, 180)); 
        movies.add(new Movie("Baahubali", "Telugu", Arrays.asList("9:00 AM", "1:00 PM", "5:00 PM"), 60,120));
        movies.add(new Movie("Dangal", "Hindi", Arrays.asList("8:30 AM", "12:30 PM", "4:30 PM"), 40, 190)); 
        movies.add(new Movie("Mersal", "Tamil", Arrays.asList("11:00 AM", "3:00 PM", "7:00 PM"), 30, 170)); 
        movies.add(new Movie("Sultan", "Hindi", Arrays.asList("10:30 AM", "2:30 PM", "6:30 PM"), 50, 200)); 
        movies.add(new Movie("Jai Bhim", "Tamil", Arrays.asList("12:00 PM", "4:00 PM", "8:00 PM"), 80, 160)); 
        movies.add(new Movie("Court", "Telugu", Arrays.asList("9:30 AM", "1:30 PM", "5:30 PM"), 50, 150)); 

        while (true) {
            System.out.println("\n--- Movie Ticket Booking System ---");
            System.out.println("1. Register");
            System.out.println("2. Login");
            System.out.println("3. Admin Panel");
            System.out.println("4. Exit");
            System.out.print("Choose an option: ");
            int choice = sc.nextInt();
            sc.nextLine();

            switch (choice) {
                case 1 -> registerUser();
                case 2 -> loginUser();
                case 3 -> adminPanel();
                case 4 -> {
                    System.out.println("Thank you for using the system!");
                    return;
                }
                default -> System.out.println("Invalid choice.");
            }
        }
    }

    static void registerUser() {
        System.out.print("Enter username: ");
        String u = sc.nextLine();
        System.out.print("Enter password: ");
        String p = sc.nextLine();
        users.add(new User(u, p));
        System.out.println("✅ Registered successfully!");
    }

    static void loginUser() {
        System.out.print("Username: ");
        String u = sc.nextLine();
        System.out.print("Password: ");
        String p = sc.nextLine();

        for (User user : users) {
            if (user.login(u, p)) {
                currentUser = user;
                System.out.println("✅ Login successful.");
                userMenu();
                return;
            }
        }
        System.out.println("❌ Login failed.");
    }

    static void userMenu() {
        while (true) {
            System.out.println("\n--- User Menu ---");
            System.out.println("1. View Movies");
            System.out.println("2. Book Ticket");
            System.out.println("3. View Booking History");
            System.out.println("4. Logout");
            System.out.print("Choose an option: ");
            int ch = sc.nextInt();
            sc.nextLine();

            switch (ch) {
                case 1 -> listMovies();
                case 2 -> bookTicket();
                case 3 -> currentUser.viewBookings();
                case 4 -> {
                    currentUser = null;
                    return;
                }
                default -> System.out.println("Invalid option.");
            }
        }
    }

    static void listMovies() {
        if (movies.isEmpty()) {
            System.out.println("No movies available. Please check later.");
            return;
        }
        System.out.println("\nAvailable Movies:");
        for (int i = 0; i < movies.size(); i++) {
            Movie m = movies.get(i);
            System.out.println((i + 1) + ". " + m.title + " (" + m.language + ") - Showtimes: " + m.showtimes + " - Price: ₹" + m.price + " - Seats left: " + m.availableSeats);
        }
    }

    static void bookTicket() {
        if (movies.isEmpty()) {
            System.out.println("No movies available to book.");
            return;
        }

        listMovies();
        System.out.print("Select movie (1-" + movies.size() + "): ");
        int movieIndex = sc.nextInt() - 1;
        sc.nextLine();
        if (movieIndex < 0 || movieIndex >= movies.size()) {
            System.out.println("Invalid movie choice.");
            return;
        }

        Movie movie = movies.get(movieIndex);

        if (movie.availableSeats == 0) {
            System.out.println("❌ No seats available for this movie.");
            return;
        }

        System.out.println("Showtimes:");
        for (int i = 0; i < movie.showtimes.size(); i++) {
            System.out.println((i + 1) + ". " + movie.showtimes.get(i));
        }

        System.out.print("Choose showtime: ");
        int showIndex = sc.nextInt() - 1;
        sc.nextLine();

        if (showIndex < 0 || showIndex >= movie.showtimes.size()) {
            System.out.println("Invalid showtime.");
            return;
        }

        System.out.print("Enter payment amount (₹" + movie.price + "): ");
        double amount = sc.nextDouble();
        if (amount < movie.price) {
            System.out.println("❌ Payment failed. Minimum is ₹" + movie.price + ".");
            return;
        }

        movie.availableSeats--;
        String bookingInfo = "Movie: " + movie.title + " (" + movie.language + "), Showtime: " + movie.showtimes.get(showIndex) + ", Amount Paid: ₹" + movie.price;
        currentUser.addBooking(bookingInfo);
        System.out.println("✅ Booking Confirmed!");
        System.out.println(bookingInfo);
    }

    static void adminPanel() {
        System.out.print("Admin Password: ");
        String pass = sc.nextLine();
        if (!pass.equals("admin123")) {
            System.out.println("❌ Access Denied.");
            return;
        }

        while (true) {
            System.out.println("\n--- Admin Panel ---");
            System.out.println("1. Add Movie");
            System.out.println("2. View All Movies");
            System.out.println("3. Exit Admin Panel");
            System.out.print("Choose option: ");
            int ch = sc.nextInt();
            sc.nextLine();

            switch (ch) {
                case 1 -> addMovie();
                case 2 -> listMovies();
                case 3 -> { return; }
                default -> System.out.println("Invalid option.");
            }
        }
    }

    static void addMovie() {
        System.out.print("Enter movie title: ");
        String title = sc.nextLine();
        System.out.print("Enter movie language (e.g., Tamil, Telugu, Hindi): ");
        String language = sc.nextLine();
        System.out.print("Enter showtimes (comma separated): ");
        String[] times = sc.nextLine().split(",");
        System.out.print("Enter available seats: ");
        int seats = sc.nextInt();
        System.out.print("Enter price: ");
        double price = sc.nextDouble();
        sc.nextLine();

        movies.add(new Movie(title, language, Arrays.asList(times), seats, price));
        System.out.println("✅ Movie added.");
    }
}
