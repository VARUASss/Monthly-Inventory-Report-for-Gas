# Monthly-Inventory-Report-for-Gas
Monthly Inventory Report for Gas



/**
 * Creates a Monthly Inventory Record for the month.
 * Bugs: if user inputs a date that does not exit, there is an ArrayIndexOutOfBounds
 * @author Siddhartha Saurav Regmi AKA VARUASss
 * version: 1.0
 *
 *
 */

import java.io.File;
import java.io.FileNotFoundException;
import java.io.PrintWriter;
import java.util.Scanner;

public class Sales {
    private double[] soldGallons = null;
    private double[] shortOrOver = null;
    private double totalSO;
    private double totalSales;
    private int month;
    private int days;
    private int year;
    private int[] gallonsReceivedOnDay = null;
    private int[] daysArray = null;
    int[] start = null;
    int[] end = null;
    Scanner scan1 = new Scanner(System.in);
    Scanner scan2 = new Scanner(System.in);
    Scanner scan = new Scanner(System.in);

    Sales() {
        this.month = 0;
        this.days = 0;
        this.year = 0;
    }

    public void setDays(int n) {
        this.days = n;
    }

    public int getDays() {
        return this.days;
    }


    //ask for month and year


    /**
     * Ask the user for the month and year if february
     */
    public void question() {

        System.out.println("What is the month? (1-12)");
        this.month = scan.nextInt();

        if (month == 1 || month == 3 || month == 5 || month == 7 || month == 8 || month == 10 || month == 12) {

            setDays(31);

        }
        if (month == 4 || month == 6 || month == 9 || month == 11) {

            setDays(30);

        }
        if (month == 2) {
            System.out.println("What year is it.");
            this.year = scan.nextInt();
            if (year % 4 == 0) {
                setDays(29);
            } else {
                setDays(28);
            }
        }
    }
    //find on which day they received and how much they received


    /**
     * ask for when they received gas and how much they received
     */
    public void received() {

        daysArray = new int[getDays()];
        gallonsReceivedOnDay = new int[getDays()];
        try {

            for (int i = 0; i < daysArray.length; i++) {
                this.daysArray[i] = i + 1;
            }


            for (int i = 0; i < gallonsReceivedOnDay.length - 1; i++) {
                System.out.println("Enter the day when you received gas. Enter -1 to stop.");
                int d = scan1.nextInt();
                if (d == -1) {
                    break;
                }
                System.out.println("Enter how much you received");
                int g = scan2.nextInt();
                gallonsReceivedOnDay[d - 1] = g;


                String undo = "undo";
                System.out.println("Enter 'undo' if you need to undo or redo this day.");
                if(scan1.next().equalsIgnoreCase(undo)){
                    i = i - 2;
                }


            }

        } catch (ArrayIndexOutOfBoundsException e) {
            e.printStackTrace();
        }


    }

    //start and end

    /**
     * ask for start and end each day
     */
    public void inventory() {

        start = new int[daysArray.length];
        end = new int[daysArray.length];
        //find start
        for (int i = 0; i < daysArray.length; i++) {
            if (daysArray[i] != daysArray.length) {
                System.out.println("Enter your start stick on day " + daysArray[i]);
                start[i] = scan1.nextInt();
            } else if (daysArray[i] == daysArray.length) {
                System.out.println("Enter your start stick on day " + daysArray[i]);
                start[i] = scan1.nextInt();
            }
            String undo = "undo";
            System.out.println("Enter 'undo' if you need to undo or redo this day.");
            if(scan1.next().equalsIgnoreCase(undo)){
                i = i - 2;
            }
        }
        //find end
        for (int i = 0; i < daysArray.length; i++) {
            if (daysArray[i] != daysArray.length) {
                end[i] = start[i + 1];
            } else if (daysArray[i] == daysArray.length) {
                System.out.println("Enter your end stick on day " + daysArray[i]);
                end[i] = scan2.nextInt();
                String undo = "undo";
                System.out.println("Enter 'undo' if you need to undo or redo this day.");
                if(scan1.next().equalsIgnoreCase(undo)){
                    i = i - 2;
                }
            }
        }

    }


    /**
     * asks for how the gas on that day and how much they sold assuming 50cents in profit per gallon and calculates a total
     */
    public void soldGallons() {
        double[] plus = new double[daysArray.length];
        double[] gas = new double[daysArray.length];
        soldGallons = new double[daysArray.length];
        for (int i = 0; i < daysArray.length; i++) {
            System.out.println("Enter your plus on day " + daysArray[i]);
            double a = scan1.nextDouble();
            plus[i] = a;
            System.out.println("Enter your gas on day " + daysArray[i]);
            double b = scan1.nextDouble();
            gas[i] = b;
            soldGallons[i] = ((plus[i] / 2.) + gas[i]);
            String undo = "undo";
            System.out.println("Enter 'undo' if you need to undo or redo this day.");
            if(scan1.next().equalsIgnoreCase(undo)){
                i = i - 2;
            }

            totalSales = totalSales + soldGallons[i];
        }

    }

    /**
     * find if they were short or over per day and on the month
     */
    public void findShortOrOver() {
        shortOrOver = new double[daysArray.length];
        totalSO = 0;
        for (int i = 0; i < daysArray.length; i++) {

            shortOrOver[i] = end[i] - (start[i] + soldGallons[i] + gallonsReceivedOnDay[i]);

            totalSO = totalSO + shortOrOver[i];
        }

    }


    /**
     * finds the sales and displays the record with total sold
     */
    public void findSales() {
        this.question();
        this.received();
        this.inventory();
        this.soldGallons();
        this.findShortOrOver();

        //read

        for (int i = 0; i < this.daysArray.length; i++) {
            System.out.printf("Day: %-10d Start: %-10d Received %-10d Sold: %-15f End: %-10d Shot/Over: %-10f %n \n",
                    this.daysArray[i], this.start[i], this.gallonsReceivedOnDay[i], this.soldGallons[i], this.end[i], this.shortOrOver[i]);
        }

        System.out.println("You are short/over: " + this.totalSO);
        System.out.println();
        System.out.println("Your total sales for the month was: " + this.totalSales);
        PrintWriter printWriter = null;
        try {
            String sales = "Sales";
            File file = new File(sales + ".txt");
            printWriter = new PrintWriter(file);
            for (int i = 0; i < this.daysArray.length; i++) {
                printWriter.format("Day: %-10d Start: %-10d Received %-10d Sold: %-15f End: %-10d Shot/Over: %-10f %n \n",
                        this.daysArray[i], this.start[i], this.gallonsReceivedOnDay[i], this.soldGallons[i], this.end[i], this.shortOrOver[i]);
            }
           printWriter.println("You are short/over: " + this.totalSO);
           printWriter.println();
           printWriter.println("Your total sales for the month was: " + this.totalSales);

        } catch (FileNotFoundException e) {
            e.printStackTrace();
        } finally {
            if (printWriter != null) {
                printWriter.close();
            }
        }
    }
        public static void main (String[]args){
            Sales sales = new Sales();
            sales.findSales();
        }
    }
