# Atm-Project1
Java-based ATM system with console interface for balance inquiry, cash withdrawal, deposit, and mini statement. Features PIN verification, low balance alerts, and transaction history. Organized into separate classes and interfaces, demonstrating basic Java programming principles.

import java.util.HashMap;
import java.util.Map;
import java.util.Scanner;

interface AtmOperationInterf {
    void viewBalance();
    void withdrawAmount(double withdrawAmount);
    void depositAmount(double depositAmount);
    void viewMiniStatement();
}

class ATM {
    private double balance;
    private double depositAmount;
    private double withdrawAmount;

    public double getBalance() {
        return balance;
    }

    public void setBalance(double balance) {
        this.balance = balance;
    }

    public double getDepositAmount() {
        return depositAmount;
    }

    public void setDepositAmount(double depositAmount) {
        this.depositAmount = depositAmount;
    }

    public double getWithdrawAmount() {
        return withdrawAmount;
    }

    public void setWithdrawAmount(double withdrawAmount) {
        this.withdrawAmount = withdrawAmount;
    }
}

class AtmOperationImpl implements AtmOperationInterf {
    ATM atm = new ATM();
    Map<Double, String> ministmt = new HashMap<>();

    public AtmOperationImpl() {
        atm.setBalance(10000.0); // Initial balance
    }

    @Override
    public void viewBalance() {
        System.out.println("Available Balance is : " + atm.getBalance());
    }

    @Override
    public void withdrawAmount(double withdrawAmount) {
        if (withdrawAmount % 500 == 0) {
            if (withdrawAmount <= atm.getBalance()) {
                ministmt.put(withdrawAmount, " Amount Withdrawn");
                System.out.println("Collect the Cash " + withdrawAmount);
                atm.setBalance(atm.getBalance() - withdrawAmount);
                viewBalance();
            } else {
                System.out.println("Insufficient Balance!!");
            }
        } else {
            System.out.println("Please enter the amount in multipal of 500");
        }
    }

    @Override
    public void depositAmount(double depositAmount) {
        ministmt.put(depositAmount, " Amount Deposited");
        System.out.println(depositAmount + " Deposited Successfully!!");
        atm.setBalance(atm.getBalance() + depositAmount);
        viewBalance();
    }

    @Override
    public void viewMiniStatement() {
        for (Map.Entry<Double, String> m : ministmt.entrySet()) {
            System.out.println(m.getKey() + "" + m.getValue());
        }
    }
}

public class Main {
    public static void main(String[] args) {
        AtmOperationInterf op = new AtmOperationImpl();
        int atmnumber = 12345;
        int atmpin = 123;
        Scanner in = new Scanner(System.in);
        System.out.println("Welcome to ATM Machine!!!");
        System.out.print("Enter Atm Number : ");
        int atmNumber = in.nextInt();
        System.out.print("Enter Pin: ");
        int pin = in.nextInt();
        if ((atmnumber == atmNumber) && (atmpin == pin)) {
            while (true) {
                System.out.println("1.View Available Balance\n2.Withdraw Amount\n3.Deposit Amount\n4.View Ministatement\n5.Exit");
                System.out.println("Enter Choice : ");
                int ch = in.nextInt();
                if (ch == 1) {
                    op.viewBalance();
                } else if (ch == 2) {
                    System.out.println("Enter amount to withdraw ");
                    double withdrawAmount = in.nextDouble();
                    op.withdrawAmount(withdrawAmount);
                } else if (ch == 3) {
                    System.out.println("Enter Amount to Deposit :");
                    double depositAmount = in.nextDouble();//5000
                    op.depositAmount(depositAmount);
                } else if (ch == 4) {
                    op.viewMiniStatement();
                } else if (ch == 5) {
                    System.out.println("Collect your ATM Card\n Thank you for using ATM Machine!!");
                    System.exit(0);
                } else {
                    System.out.println("Please enter correct choice");
                }
                
                // Automatic feature: Check for low balance
                if (op instanceof AtmOperationImpl) {
                    AtmOperationImpl atmOp = (AtmOperationImpl) op;
                    ATM atm = atmOp.atm;
                    if (atm.getBalance() < 1000) {
                        System.out.println("Low balance alert! Your current balance is " + atm.getBalance());
                    }
                }
            }
        } else {
            System.out.println("Incorrect Atm Number or pin");
            System.exit(0);
        }
    }
}
