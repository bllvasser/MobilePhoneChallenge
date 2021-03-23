# MobilePhoneChallenge
package com.company;


public class Contact {

    private String name;
    private String number;

    public Contact(String name, String number) {
        this.name = name;
        this.number = number;
    }

    public String getNumber() {

        return number;
    }

    public String getName() {

        return name;
    }

    public static Contact createContact(String name, String phoneNumber) {

        return new Contact(name, phoneNumber);
    }

}

package com.company;

import java.util.ArrayList;

public class MobilePhone {
    private String myNumber;
    private ArrayList<Contact> myContacts;

    public MobilePhone(String myNumber) {
        this.myNumber = myNumber;
        this.myContacts = new ArrayList<Contact>();


    }

    public boolean addNewContact(Contact contact) {
        if (findContact(contact.getName()) >= 0) {
            System.out.println("Contact is already on file");
            return false;
        }
        myContacts.add(contact);
        return true;
    }


    public void printContact() {
        System.out.println("Contact List");
        for (int i = 0; i < this.myContacts.size(); i++) {
            System.out.println((i + 1) + "." + this.myContacts.get(i).getName() + "->" + this.myContacts.get(i).getNumber());
        }
    }

    public boolean updateContact(Contact oldContact, Contact newContact) {
        int foundposition = findContact(oldContact);
        if (foundposition < 0) {
            System.out.println(oldContact + "was not found");
            return false;
        } else if (findContact(newContact.getName()) != -1) {
            System.out.println("Contact with name " + newContact.getName() + "already exists. Update was not successful");
            return false;
        }
        this.myContacts.set(foundposition, newContact);
        System.out.println((oldContact.getName() + "was replaced with " + newContact.getName()));
        return true;
    }

    public boolean removeContact(Contact contact) {
        int foundPosition = findContact(contact);
        if (foundPosition < 0) {
            System.out.println(contact.getName() + ", was not found");
            return false;
        }
        this.myContacts.remove(foundPosition);
        System.out.println(contact.getName() + ", was deleted");
        return true;
    }

    private int findContact(Contact contact) {

        return this.myContacts.indexOf(contact);
    }

    private int findContact(String contactName) {
        for (int i = 0; i < this.myContacts.size(); i++) {
            Contact contact = this.myContacts.get(i);
            if (contact.getName().equals(contactName)) {
                return i;
            }
        }
        return -1;
    }

    public String queryContact(Contact contact) {
        if (findContact(contact) >= 0) {
            return contact.getName();
        }
        return null;
    }

    public Contact queryContact(String name) {
        int position = findContact(name);
        if (position >= 0) {
            return this.myContacts.get(position);
        }
        return null;
    }
}

package com.company;

import java.util.ArrayList;
import java.util.Random;
import java.util.Scanner;

public class Main {

    private static Scanner scanner = new Scanner(System.in);


    private static MobilePhone mobilePhone = new MobilePhone("1-800-fakenumber");

    public static void main(String[] args) {


        boolean quit = false;
        startPhone();
        printInstructions();
        while (!quit) {
            System.out.println("\nEnter action: (6 to show available actions)");
            int action = scanner.nextInt();
            scanner.nextLine();

            switch (action) {
                case 0:
                    System.out.println("\n shutting down...");
                    break;
                case 1:
                    mobilePhone.printContact();
                case 2:
                    addNewContact();
                    break;
                case 3:
                    updateContacts();
                    break;
                case 4:
                    removeCotacts();
                    break;
                case 5:
                    queryContacts();
                    break;
                case 6:
                    quit = true;
                    break;
            }

        }

    }

    private static void removeCotacts() {
        System.out.println("Enter existing contact name");
        String name = scanner.nextLine();
        Contact existingContactRecord = mobilePhone.queryContact(name);
        if (existingContactRecord == null) {
            System.out.println("Contact not found");
            return;
        }
        if (mobilePhone.removeContact(existingContactRecord)) {
            System.out.println("Successfully deleted");
        } else {
            System.out.println("Error deleting contact");
        }
    }

    private static void queryContacts() {
        System.out.println("Enter existing contact name");
        String name = scanner.nextLine();
        Contact existingContactRecord = mobilePhone.queryContact(name);
        if (existingContactRecord == null) {
            System.out.println("Contact not found");
            return;
        }
        System.out.println("Name: " + existingContactRecord.getName() + "phone numbers is " + existingContactRecord.getNumber());
    }

    private static void printInstructions() {

        System.out.println("\n Available actions: \npress");
        System.out.println("0 - to shutdown/n" +
                "1 - to print contacts\n" +
                "2 - to add a new contact\n" +
                "3 - to update an existing contact/n" +
                "4 - to remove an existing contct\n" +
                "5 - to query if an existing contact exists\n" +
                "6 - to print a list of available actions. ");
        System.out.println("Choose your actions");

    }

    private static void startPhone() {
        System.out.println("Phone is starting...");
    }

    private static void addNewContact() {
        System.out.println("Enter new contact name");
        String name = scanner.nextLine();
        System.out.println("Enter phone number");
        String number = scanner.nextLine();
        Contact newContact = Contact.createContact(name, number);
        if (mobilePhone.addNewContact(newContact)) {
            System.out.println("New contact added: name " + name + ", phone = " + number);
        } else {
            System.out.println(("Cannot add, " + name + ", phone " + number));
        }
    }

    private static void updateContacts() {
        System.out.println("Enter existing contact name: ");
        String name = scanner.nextLine();
        Contact existingContactRecord = mobilePhone.queryContact(name);
        if (existingContactRecord == null) {
            System.out.println("Contact not found");
            return;
        }
        System.out.print("Enter new contact name");
        String newName = scanner.nextLine();
        System.out.println("Enter phone number");
        String newPhoneNumber = scanner.nextLine();
        Contact newContact = Contact.createContact(name, newPhoneNumber);
        if (mobilePhone.updateContact(existingContactRecord, newContact)) {
            System.out.println("Successfully updated record");
        } else {
            System.out.println("Error updating contact");
        }
    }


}

/Library/Java/JavaVirtualMachines/amazon-corretto-11.jdk/Contents/Home/bin/java "-javaagent:/Applications/IntelliJ IDEA CE.app/Contents/lib/idea_rt.jar=60897:/Applications/IntelliJ IDEA CE.app/Contents/bin" -Dfile.encoding=UTF-8 -classpath /Users/Bilaal/IdeaProjects/ArrayListChallenge/out/production/ArrayListChallenge com.company.Main
Phone is starting...

 Available actions: 
press
0 - to shutdown/n1 - to print contacts
2 - to add a new contact
3 - to update an existing contact/n4 - to remove an existing contct
5 - to query if an existing contact exists
6 - to print a list of available actions. 
Choose your actions

Enter action: (6 to show available actions)




