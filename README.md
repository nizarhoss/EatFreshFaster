//# EatFreshFaster






import javax.swing.*;
import javax.swing.event.*;
import java.awt.*;
import java.awt.event.*;

/**
   The OrderCalculatorGUI class creates the GUI for the
   SubwayOrderTaker application.
*/

public class OrderCalculatorGUI extends JFrame
{
   private BreadPanel bread;     // Bagel panel
   private CheesePanel cheese; // Topping panel
   private MeatPanel meat; // Topping panel
   private VeggiePanel veggies; // Topping panel
   private SaucePanel sauce; // Topping panel
   private ExtrasPanel extras; // Topping panel
   private GreetingPanel banner;  // To display a greeting

   private JPanel startPanel;		// Start panel
   private JTabbedPane mainPanel;	// Main panel
   private JPanel buttonPanel;    // To hold the buttons
   private JPanel checkoutPanel;    // To hold the buttons
   private JButton exitButton;    // To exit the application
   private JButton previousButton;// To go to previous tab
   private JButton nextButton;	  // To go to next tab
   private JButton startButton;
   private JTextArea textArea;

// Constants for set up of the window
   public static final int WIDTH = 600;
   public static final int HEIGHT = 600;
   private static final int TABS = 7; // number of tabs in main panel
   private final double TAX_RATE = 0.06; // Sales tax rate
   /**
      Constructor
   */

   public OrderCalculatorGUI()
   {
      // Display a title.
      setTitle("Order Calculator");

      //Sets default size of window
      setSize(WIDTH, HEIGHT);
      
      // Specify an action for the close button.
      setDefaultCloseOperation(JFrame.DISPOSE_ON_CLOSE);	// program keeps running until last orderCalc window is closed

      // Create a BorderLayout manager.
      setLayout(new BorderLayout());

      // Create the custom panels.
      banner = new GreetingPanel();
      bread = new BreadPanel();
      cheese = new CheesePanel();
      meat = new MeatPanel();
      veggies = new VeggiePanel();
      sauce = new SaucePanel();
      extras = new ExtrasPanel();

      // Create the start, button, main, and checkout panels.
      buildStartPanel();
      buildButtonPanel();
      buildCheckoutPanel();
      buildMainPanel();

      // Add the components to the content pane.
      add(banner, BorderLayout.NORTH);
      add(startPanel, BorderLayout.CENTER);

      // Display the contents of the window . 
      setVisible(true);
   }

   /**
      The buildStartPanel method builds the starting panel.
   */
   private void buildStartPanel()
   {
	   setLayout(new BorderLayout());
	   
	   startPanel = new JPanel();			
	   startButton = new JButton("Start Order");
	   startButton.addActionListener(new StartButtonListener());
	   startPanel.add(startButton, BorderLayout.CENTER);
   }
   /**
   The buildMainPanel method builds the tabbed pane.
    */
   private void buildMainPanel()
   {
	  mainPanel = new JTabbedPane(JTabbedPane.RIGHT);	//creates TabbedPane, 
  
 	  mainPanel.addTab("Bread", null, bread);	//creates first tab using bread panel	  
 	  mainPanel.addTab("Cheese", null, cheese);
 	  mainPanel.addTab("Meat", null, meat);
 	  mainPanel.addTab("Veggies", null, veggies);
 	  mainPanel.addTab("Sauce", null, sauce);
 	  mainPanel.addTab("Extras", null, extras);
 	  mainPanel.addTab("Checkout", null, checkoutPanel);
	  mainPanel.addChangeListener(new TabListener());
   }
   /**
   The buildButtonPanel method builds the button panel.
    */
   private void buildButtonPanel()
   {
      // Create a panel for the buttons.
      buttonPanel = new JPanel();

      // Create the buttons.      
      previousButton = new JButton("Previous");
      nextButton = new JButton("Next");

      exitButton = new JButton("Cancel Order");

      // Register the action listeners.
      previousButton.addActionListener(new PreviousButtonListener());
      nextButton.addActionListener(new NextButtonListener());      
      exitButton.addActionListener(new ExitButtonListener());

      // Add the buttons to the button panel.
      buttonPanel.add(previousButton);
      buttonPanel.add(nextButton);
      buttonPanel.add(exitButton);
      previousButton.setEnabled(false);
   }
   /**
   The buildCheckoutPanel method builds the checkout panel.
    */
   private void buildCheckoutPanel()	//work in progress
   {
      // Create a panel for the buttons.
      checkoutPanel = new JPanel();      
      
      textArea = new JTextArea(30, 30);
      JScrollPane scrollPane = new JScrollPane(textArea); 
      textArea.setEditable(false);
      
      setLayout(new BorderLayout());
      checkoutPanel.add(textArea, BorderLayout.CENTER);

      
      // Calculate the subtotal.
      updateCheckout(bread.getBreadCost(), bread.getBreadType(), 
 		  		cheese.getCheeseCost(), cheese.getCheeseType(),
 		  		meat.getMeatCost(), meat.getMeatType(),
 		  		veggies.getVeggieCost(), veggies.getVeggieType(),
 		  		sauce.getSauceCost(), sauce.getSauceType(),
 		  		extras.getExtrasCost(), extras.getExtrasType());
   }
   /**
      Private inner class that handles the event when
      the user clicks the Calculate button.
   */

   private class CalcButtonListener implements ActionListener
   {
      public void actionPerformed(ActionEvent e)
      {
    	  
    	  mainPanel.setSelectedIndex(TABS-1);
      }
   }

   /**
      Private inner class that handles the event when
      the user clicks the Exit button.
   */

   private class ExitButtonListener implements ActionListener
   {
      public void actionPerformed(ActionEvent e)
      {
    	  new OrderCalculatorGUI();		//opens new order calculator window
    	  dispose();					//closes old window
      }
   }
   
   /**
   Private inner class that handles the event when
   the user clicks the Previous button.
    */
   private class PreviousButtonListener implements ActionListener
   {
      public void actionPerformed(ActionEvent e)
      {
    	  mainPanel.setSelectedIndex( mainPanel.getSelectedIndex()-1);	
      }
   }
   /**
   Private inner class that handles the event when
   the user clicks the Next button.
    */
   private class NextButtonListener implements ActionListener
   {
      public void actionPerformed(ActionEvent e)
      {
    	  mainPanel.setSelectedIndex( mainPanel.getSelectedIndex()+1);
    	  
      }
   }
   /**
   Private inner class that handles the event when
   the user clicks the Start button.
    */
   private class StartButtonListener implements ActionListener
   {
      public void actionPerformed(ActionEvent e)
      {
    	  remove(startPanel);						//starts ordering process by adding the button panel to the bottom of the
    	  add(mainPanel, BorderLayout.CENTER);		//window, removing the start panel and replacing it with the main panel.
    	  add(buttonPanel, BorderLayout.SOUTH);		
    	  revalidate();								//refreshes the frame to show changes
      }
   }
   
   private class TabListener implements ChangeListener
   {
	   public void stateChanged(ChangeEvent e) 
	   {
		   updateCheckout(bread.getBreadCost(), bread.getBreadType(), 
   		  		cheese.getCheeseCost(), cheese.getCheeseType(),
   		  		meat.getMeatCost(), meat.getMeatType(),
   		  		veggies.getVeggieCost(), veggies.getVeggieType(),
   		  		sauce.getSauceCost(), sauce.getSauceType(),
   		  		extras.getExtrasCost(), extras.getExtrasType());
		   
		   if(mainPanel.getSelectedIndex() >= TABS - 1)		
		   	   nextButton.setEnabled(false);				//disables the "next" button when tab 1 is selected
		   else
			   nextButton.setEnabled(true);					//enables the "next" button when tab 1 is not selected
			   
	   	   if(mainPanel.getSelectedIndex() <= 0)		
	   		   previousButton.setEnabled(false);		//disables the "previous" button when the last tab is selected
	   	   else
	   		previousButton.setEnabled(true);	   	   	//enables the "previous" button when the last tab is not selected
	   }   
   }
   private void updateCheckout(double breadCost, String breadType,
		   						double cheese, String cheeseType, 
		   						double meat, String meatType,
		   						double veggies , String veggieType,
		   						double sauce, String sauceType,
		   						double extras, String extrasType)
   {
	// Variables to hold the subtotal, tax, and total
	      double subtotal, tax, total;
	      
	      // Calculate the subtotal.
	      subtotal = breadCost + cheese + meat + veggies + sauce + extras;
	      tax = subtotal * TAX_RATE;
	      total = subtotal + tax;
	      
	      // Display the charges.
	      
	      textArea.setText(String.format(breadType + "\n" + cheeseType + "\n" + meatType + "\n"
	    		  						+ veggieType + "\n"+ sauceType + "\n"+ extrasType + "\n"));
	      
	      textArea.append(String.format("Subtotal: $%,.2f\n" 
	    		  						+ "Tax: $%,.2f\n" 
	    		  						+ "Total: $%,.2f", subtotal, tax, total));
	      
   }
   
  
   /**
      main method
   */
   
   public static void main(String[] args)
   {
      new OrderCalculatorGUI();
      
   }
}





