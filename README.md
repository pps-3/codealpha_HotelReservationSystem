# codealpha_HotelReservationSystem
import java.util.HashMap;
import java.util.Map;
import java.util.Scanner;

public class HotelReservationSystem {
    private static final String[] ROOM_NUMBERS = {"101", "102", "103", "104", "105"};
    private Map<String, RoomDetails> rooms;
    private Map<String, ReservationDetails> reservations;
    @SuppressWarnings({ "rawtypes", "unchecked" })
    public HotelReservationSystem() {
        rooms = new HashMap();
        reservations = new HashMap();
        initializeRooms();
    }
    private class RoomDetails {
        private String roomNumber;
        private String category;
        private double price;
        private boolean available;

        public RoomDetails(String roomNumber, String category, double price, boolean available) {
            this.roomNumber = roomNumber;
            this.category = category;
            this.price = price;
            this.available = available;
        }
    }
    private class ReservationDetails {
        private String roomNumber;
        private String guestName;
        private String checkInDate;
        private String checkOutDate;
        private double totalPrice;

        public ReservationDetails(String roomNumber, String guestName, String checkInDate, String checkOutDate, double totalPrice) {
            this.roomNumber = roomNumber;
            this.guestName = guestName;
            this.checkInDate = checkInDate;
            this.checkOutDate = checkOutDate;
            this.totalPrice = totalPrice;
        }
    }
    private void initializeRooms() {
        rooms.put(ROOM_NUMBERS[0], new RoomDetails(ROOM_NUMBERS[0], "Deluxe", 150.0, true));
        rooms.put(ROOM_NUMBERS[1], new RoomDetails(ROOM_NUMBERS[1], "Standard", 100.0, true));
        rooms.put(ROOM_NUMBERS[2], new RoomDetails(ROOM_NUMBERS[2], "Suite", 200.0, true));
        rooms.put(ROOM_NUMBERS[3], new RoomDetails(ROOM_NUMBERS[3], "Economy", 80.0, true));
        rooms.put(ROOM_NUMBERS[4], new RoomDetails(ROOM_NUMBERS[4], "Deluxe", 150.0, true));
    }
    public void searchAvailableRooms(String category) {
        System.out.println("Available rooms in the " + category + " category:");
        for (RoomDetails roomDetails : rooms.values()) {
            if (roomDetails.category.equalsIgnoreCase(category) && roomDetails.available) {
                System.out.println("Room Number: " + roomDetails.roomNumber + ", Price: $" + roomDetails.price);
            }
        }
    }
    public void makeReservation(String roomNumber, String guestName, String checkInDate, String checkOutDate) {
        if (rooms.containsKey(roomNumber) && rooms.get(roomNumber).available) {
            RoomDetails roomDetails = rooms.get(roomNumber);
            double totalPrice = (double) (getDaysBetween(checkInDate, checkOutDate) + 1) * roomDetails.price;
            ReservationDetails reservationDetails = new ReservationDetails(roomNumber, guestName, checkInDate, checkOutDate, totalPrice);
            reservations.put(roomNumber, reservationDetails);
            roomDetails.available = false;
            System.out.println("Reservation made successfully. Total price: $" + totalPrice);
        } else {
            System.out.println("Room not available or not found.");
        }
    }
    public void viewReservation(String roomNumber) {
        if (reservations.containsKey(roomNumber)) {
            ReservationDetails reservationDetails = reservations.get(roomNumber);
            System.out.println("Room Number: " + reservationDetails.roomNumber);
            System.out.println("Guest Name: " + reservationDetails.guestName);
            System.out.println("Check-in Date: " + reservationDetails.checkInDate);
            System.out.println("Check-out Date: " + reservationDetails.checkOutDate);
            System.out.println("Total Price: $" + reservationDetails.totalPrice);
        } else {
            System.out.println("No reservation found for the given room number.");
        }
    }
    private int getDaysBetween(String checkInDate, String checkOutDate) {
        return 2;
    }

    public static void main(String[] args) {
        HotelReservationSystem system = new HotelReservationSystem();
        Scanner scanner = new Scanner(System.in);

        int choice;
        do {
            System.out.println("\nHotel Reservation System");
            System.out.println("1. Search Available Rooms");
            System.out.println("2. Make Reservation");
            System.out.println("3. View Reservation");
            System.out.println("4. Exit");
            System.out.print("Enter your choice (1-4): ");
            choice = scanner.nextInt();
            scanner.nextLine(); 

            switch (choice) {
                case 1:
                    System.out.println("Available room categories:");
                    System.out.println("1. Deluxe");
                    System.out.println("2. Standard");
                    System.out.println("3. Suite");
                    System.out.println("4. Economy");
                    System.out.print("Enter your choice (1-4): ");
                    int categoryChoice = scanner.nextInt();
                    scanner.nextLine(); 
                    String category;
                    switch (categoryChoice) {
                        case 1:
                            category = "Deluxe";
                            break;
                        case 2:
                            category = "Standard";
                            break;
                        case 3:
                            category = "Suite";
                            break;
                        case 4:
                            category = "Economy";
                            break;
                        default:
                            System.out.println("Invalid choice.");
                            continue;
                    }
                    system.searchAvailableRooms(category);
                    break;
                case 2:
                    System.out.print("Enter room number: ");
                    String roomNumber = scanner.nextLine();
                    System.out.print("Enter your name: ");
                    String guestName = scanner.nextLine();
                    System.out.print("Enter check-in date: ");
                    String checkInDate = scanner.nextLine();
                    System.out.print("Enter check-out date: ");
                    String checkOutDate = scanner.nextLine();
                    system.makeReservation(roomNumber, guestName, checkInDate, checkOutDate);
                    break;
                case 3:
                    System.out.print("Enter room number: ");
                    roomNumber = scanner.nextLine();
                    system.viewReservation(roomNumber);
                    break;
                case 4:
                    System.out.println("Exiting the Hotel Reservation System...");
                    break;
                default:
                    System.out.println("Invalid choice. Please try again.");
            }
        } while (choice != 4);
    }
}
