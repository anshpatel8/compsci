# Page for my othello cw backup

## GUIView

```
package reversi;

import java.awt.BorderLayout;
import java.awt.Color;
import java.awt.Dimension;
import java.awt.GridLayout;
import javax.swing.JButton;
import javax.swing.JFrame;
import javax.swing.JLabel;
import javax.swing.JPanel;


public class GUIView implements IView
{

	IModel model;
	IController controller;
	
	/*constructor*/
	public GUIView() 
	{
	}
	
	/*white frame creation*/
	JFrame whiteplayerframe = new JFrame();
	JLabel whitelabel = new JLabel();
	JPanel whitegrid = new JPanel(new GridLayout(8,8));
	JPanel whitebuttons = new JPanel(new GridLayout(2,1));
	/*black frame creation*/
	JFrame blackplayerframe = new JFrame();
	JLabel blacklabel = new JLabel();
	JPanel blackgrid = new JPanel(new GridLayout(8,8));
	JPanel blackbuttons = new JPanel(new GridLayout(2,1));

	
	
	@Override
	public void initialise(IModel model, IController controller) {
		
		this.model = model;
		this.controller = controller;
		
		/*white frame*/
		whiteplayerframe.setLayout(new BorderLayout());
		whiteplayerframe.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
		whiteplayerframe.setTitle("White Player");
		whiteplayerframe.setLocationRelativeTo(null);
		whitelabel.setText("white player");
		whiteplayerframe.add(whitelabel, BorderLayout.NORTH);
		whiteplayerframe.add(whitegrid, BorderLayout.CENTER);
        for (int x = 0; x <= 7; x++) {
        	for (int y = 0; y <= 7; y++) {
        		mybutton button = new mybutton(x,y);
        		whitegrid.add(button);
        	}
            
        }
        whiteplayerframe.add(whitebuttons, BorderLayout.SOUTH);
        JButton greedyai = new JButton();
        JButton Restart = new JButton();
        greedyai.setText("Greedy AI (Play white)");
        Restart.setText("Restart");
        whitebuttons.add(greedyai);
        whitebuttons.add(Restart);
		whiteplayerframe.pack();
		whiteplayerframe.setVisible(true);
		
		
		/*black frame*/
		blackplayerframe.setLayout(new BorderLayout());
		blackplayerframe.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
		blackplayerframe.setTitle("Black Player");
		blackplayerframe.setLocationRelativeTo(null);
		blacklabel.setText("black player");
		blackplayerframe.add(blacklabel, BorderLayout.NORTH);
		blackplayerframe.add(blackgrid, BorderLayout.CENTER);
        for (int x = 7; x >= 0; x--) {
        	for (int y = 7; y >= 0; y--) {
        		mybutton button2 = new mybutton(x,y);
        		blackgrid.add(button2);
        	}
            
        }
        blackplayerframe.add(blackbuttons, BorderLayout.SOUTH);
        JButton greedyai2 = new JButton();
        JButton Restart2 = new JButton();
        greedyai2.setText("Greedy AI (Play black)");
        Restart2.setText("Restart");
        blackbuttons.add(greedyai2);
        blackbuttons.add(Restart2);
		
		
		blackplayerframe.pack();
		blackplayerframe.setVisible(true);
		
	}

	@Override
	public void refreshView() {
		// TODO Auto-generated method stub
		
	}

	@Override
	public void feedbackToUser(int player, String message) {
		// TODO Auto-generated method stub
		
	}
	
	
}

```


## Mybutton

```
package reversi;

import java.awt.Color;
import java.awt.Dimension;

import javax.swing.BorderFactory;
import javax.swing.JButton;


public class mybutton extends JButton{


	Color borderColor;
	Color whiteColor;
	Color blackColor;

	
	
	@SuppressWarnings("deprecation")
	public mybutton(int x, int y) {
		this.setLabel(String.valueOf(x) +"-"+ String.valueOf(y));
		this.setBackground(Color.GREEN);
		this.setPreferredSize(new Dimension(50,50));	
		this.setBorder(BorderFactory.createLineBorder(Color.BLACK));
	}



}
```

## ReversiController