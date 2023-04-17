# Page for my othello cw backup

## GUIView

```
package reversi;

import java.awt.BorderLayout;
import java.awt.Color;
import java.awt.Graphics;
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
	
	squarehandler[][] blackButtons = new squarehandler[8][8];
	squarehandler[][] whiteButtons = new squarehandler[8][8];
	
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
        		squarehandler buttonwhite = new squarehandler(x,y, model, controller, 1);
        		whiteButtons[x][y] = buttonwhite;
        		whitegrid.add(buttonwhite);
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
        		squarehandler buttonblack = new squarehandler(x,y, model, controller, 2);
        		blackButtons[x][y] = buttonblack;
        		blackgrid.add(buttonblack);
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
		
		for(int x = 0; x <= 7; x++) {
			for(int y = 0; y <= 7; y ++) {
				int check = model.getBoardContents(x, y);
				if (check == 1) {
					whiteButtons[x][y].setDrawColor(Color.BLACK);
					whiteButtons[x][y].setFillColor(Color.WHITE);
					whiteButtons[x][y].repaint();
				}
				else if (check == 2) {
					whiteButtons[x][y].setDrawColor(Color.WHITE);
					whiteButtons[x][y].setFillColor(Color.BLACK);
					whiteButtons[x][y].repaint();
				}
				
			}
		}
		
		for(int x = 7; x >= 0 ; x--) {
			for(int y = 7; y >= 0; y --) {
				int check = model.getBoardContents(x, y);
				if (check == 1) {
					blackButtons[x][y].setDrawColor(Color.BLACK);
					blackButtons[x][y].setFillColor(Color.WHITE);
					blackButtons[x][y].repaint();
				}
				else if (check == 2) {
					blackButtons[x][y].setDrawColor(Color.WHITE);
					blackButtons[x][y].setFillColor(Color.BLACK);
					blackButtons[x][y].repaint();
				}
				
			}
		}
		
		
	}

	@Override
	public void feedbackToUser(int player, String message) {
		
		
	}
	
}

```


## squarehandler

```
package reversi;

import java.awt.Color;
import java.awt.Dimension;
import java.awt.Graphics;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import javax.swing.BorderFactory;
import javax.swing.ButtonModel;
import javax.swing.JButton;

public class squarehandler extends JButton
{


	IModel model;
	IController controller;
	int player;
	Color drawColor;
	Color fillColor;
	
	public squarehandler(int x, int y, IModel model, IController controller, int player) {
		
		this.model = model;
		this.controller = controller;
		this.player = player;
		
		//setText(String.valueOf(x) +"-"+ String.valueOf(y));
		setBackground(Color.GREEN);
		setPreferredSize(new Dimension(50,50));	
		setBorder(BorderFactory.createLineBorder(Color.BLACK));
		addActionListener(new ActionListener(){
			public void actionPerformed(ActionEvent e) {
				
				controller.squareSelected(player, x, y);
				
				
			}
		});
	}
	
	public void setDrawColor(Color drawColor)
	{
		this.drawColor = drawColor;
	}
	
	public void setFillColor(Color fillColor)
	{
		this.fillColor = fillColor;
	}
	
	@Override
	protected void paintComponent(Graphics g){
		super.paintComponent(g);

			if(drawColor != null) {
				g.setColor(drawColor);
				g.drawOval(0, 0, 46, 46);
			}
			if(fillColor != null) {
				g.setColor(fillColor);
				g.fillOval(0, 0, 46, 46);
			}

		

	}
}

```

## ReversiController

```

package reversi;

public class ReversiController implements IController {

	IModel model;
	IView view;
	
	@Override
	public void initialise(IModel model, IView view) {
		
		this.model = model;
		this.view = view;
	}

	@Override
	public void startup() {
		// Initialise board
		int width = model.getBoardWidth();
		int height = model.getBoardHeight();
		for ( int x = 0 ; x < width ; x++ ) {
			for ( int y = 0 ; y < height ; y++ ) {
				model.setBoardContents(x, y, 0);
			}
		}
		/* setting initial pieces*/
		model.setBoardContents(3, 3, 1);
		model.setBoardContents(4, 4, 1);
		model.setBoardContents(3, 4, 2);
		model.setBoardContents(4, 3, 2);
		
		/*refresh view*/
		view.refreshView();
	}

	@Override
	public void update() {
		
		
	}

	@Override
	public void squareSelected(int player, int x, int y) {
		
		model.setBoardContents(x, y, player);		
		view.refreshView();
	}

	@Override
	public void doAutomatedMove(int player) {
		
		
	}

}

```