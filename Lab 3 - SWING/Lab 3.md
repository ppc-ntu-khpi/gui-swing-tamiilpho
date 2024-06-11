# Lab 3
# Створення GUI з допомогою Swing

## На "трійку"
Я додала файл SWINGdemo.java в проект GUIdemo, ознайомилась з кодом та запустила програму:

Результат:
![image](https://github.com/ppc-ntu-khpi/gui-swing-tamiilpho/assets/120334809/13b9afe5-ab9e-4b47-8c57-01ad0d2ebcb4)

## На "п'ять"
Оновлений код:
````java
import com.mybank.domain.Bank;
import com.mybank.domain.CheckingAccount;
import com.mybank.domain.Customer;
import com.mybank.domain.SavingsAccount;
import java.awt.BorderLayout;
import java.awt.Dimension;
import java.awt.GridLayout;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import javax.swing.JButton;
import javax.swing.JComboBox;
import javax.swing.JEditorPane;
import javax.swing.JFrame;
import javax.swing.JPanel;

/**
 *
 * @author Alexander 'Taurus' Babich
 */
public class SWINGDemo {
    
    private final JEditorPane log;
    private final JButton show;
    private final JButton report;
    private final JComboBox<String> clients;
    
    public SWINGDemo() {
        log = new JEditorPane("text/html", "");
        log.setPreferredSize(new Dimension(350, 300));
        show = new JButton("Show");
        report = new JButton("Report");
        clients = new JComboBox<>();
        for (int i = 0; i < Bank.getNumberOfCustomers(); i++) {
            clients.addItem(Bank.getCustomer(i).getLastName() + ", " + Bank.getCustomer(i).getFirstName());
        }
    }
    
    private void launchFrame() {
        JFrame frame = new JFrame("MyBank clients");
        frame.setLayout(new BorderLayout());
        JPanel cpane = new JPanel();
        cpane.setLayout(new GridLayout(1, 3));
        
        cpane.add(clients);
        cpane.add(show);
        cpane.add(report);
        frame.add(cpane, BorderLayout.NORTH);
        frame.add(log, BorderLayout.CENTER);
        
        show.addActionListener(new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent e) {
                Customer current = Bank.getCustomer(clients.getSelectedIndex());
                StringBuilder custInfo = new StringBuilder("<br>&nbsp;<b><span style=\"font-size:2em;\">");
                custInfo.append(current.getLastName()).append(", ").append(current.getFirstName()).append("</span><br><hr>");
                for (int i = 0; i < current.getNumberOfAccounts(); i++) {
                    String accType = current.getAccount(i) instanceof CheckingAccount ? "Checking" : "Savings";
                    custInfo.append("&nbsp;<b>Acc Type: </b>").append(accType)
                            .append("<br>&nbsp;<b>Balance: <span style=\"color:red;\">$")
                            .append(current.getAccount(i).getBalance()).append("</span></b><br><br>");
                }
                log.setText(custInfo.toString());
            }
        });
        
        report.addActionListener(new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent e) {
                StringBuilder report = new StringBuilder("<html><h2>Customer Report</h2><hr>");
                for (int i = 0; i < Bank.getNumberOfCustomers(); i++) {
                    Customer customer = Bank.getCustomer(i);
                    report.append(customer.getLastName()).append(", ").append(customer.getFirstName()).append("<br>");
                    for (int j = 0; j < customer.getNumberOfAccounts(); j++) {
                        String accType = customer.getAccount(j) instanceof CheckingAccount ? "Checking" : "Savings";
                        report.append("&nbsp;&nbsp;&nbsp;<b>Acc Type: </b>").append(accType)
                                .append("<br>&nbsp;&nbsp;&nbsp;<b>Balance: <span style=\"color:red;\">$")
                                .append(customer.getAccount(j).getBalance()).append("</span></b><br>");
                    }
                    report.append("<br>");
                }
                report.append("</html>");
                log.setText(report.toString());
            }
        });
        
        frame.pack();
        frame.setLocationRelativeTo(null);
        frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);  
        frame.setResizable(false);
        frame.setVisible(true);        
    }
    
    public static void main(String[] args) {
        
        Bank.addCustomer("John", "Doe");
        Bank.addCustomer("Fox", "Mulder");
        Bank.addCustomer("Dana", "Scully");
        Bank.getCustomer(0).addAccount(new CheckingAccount(2000));
        Bank.getCustomer(1).addAccount(new SavingsAccount(1000, 3));
        Bank.getCustomer(2).addAccount(new CheckingAccount(1000, 500));
        
        SWINGDemo demo = new SWINGDemo();        
        demo.launchFrame();
    }
    
}

````

Результат:
![image](https://github.com/ppc-ntu-khpi/gui-swing-tamiilpho/assets/120334809/1394b217-903e-4384-92d0-beaea200de17)



[![Gitter](https://badges.gitter.im/PPC-SE-2020/OOP.svg)](https://gitter.im/PPC-SE-2020/OOP?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge)
