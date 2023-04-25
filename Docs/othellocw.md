# Page for my othello cw backup

## GUIView

```
package reversi;

import java.awt.BorderLayout;
import java.awt.Color;
import java.awt.Graphics;
import java.awt.GridLayout;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;

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
		whitelabel.setText("White Player");
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
        Restart.addActionListener(new ActionListener()
        	{ public void actionPerformed(ActionEvent e) { controller.startup(); } } );
        greedyai.addActionListener(new ActionListener()
    	{ public void actionPerformed(ActionEvent e) { controller.doAutomatedMove(1); } } );
        whitebuttons.add(greedyai);
        whitebuttons.add(Restart);
		whiteplayerframe.pack();
		whiteplayerframe.setVisible(true);
		
		
		
		
		/*black frame*/
		blackplayerframe.setLayout(new BorderLayout());
		blackplayerframe.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
		blackplayerframe.setTitle("Black Player");
		blackplayerframe.setLocationRelativeTo(null);
		blacklabel.setText("Black Player");
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
        JButton restart2 = new JButton();
        greedyai2.setText("Greedy AI (Play black)");
        blackbuttons.add(greedyai2);
        restart2.setText("Restart");
        restart2.addActionListener(new ActionListener()
    	{ public void actionPerformed(ActionEvent e) { controller.startup(); } } );
        greedyai2.addActionListener(new ActionListener()
    	{ public void actionPerformed(ActionEvent e) { controller.doAutomatedMove(2); } } );
        blackbuttons.add(restart2);
		
		
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
				else if (check == 0) {
					whiteButtons[x][y].setDrawColor(Color.GREEN);
					whiteButtons[x][y].setFillColor(Color.GREEN);
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
				else if (check == 0) {
					blackButtons[x][y].setDrawColor(Color.GREEN);
					blackButtons[x][y].setFillColor(Color.GREEN);
					blackButtons[x][y].repaint();
				}
				
			}
		}
		
		
	}

	@Override
	public void feedbackToUser(int player, String message) {
		
		if ( player == 1 )
			whitelabel.setText(message);
		else if ( player == 2 )
			blacklabel.setText(message);
		
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
		
		//setText(String.valueOf(x) +"x-y"+ String.valueOf(y));
		setBackground(Color.GREEN);
		setPreferredSize(new Dimension(50,50));	
		setBorder(BorderFactory.createLineBorder(Color.BLACK));
		addActionListener(new ActionListener(){
			public void actionPerformed(ActionEvent e) {
				if(model.hasFinished() == false) {
					controller.squareSelected(player, x, y);
				}
				
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
	boolean whiteToPlay = true, blackToPlay = false;
	
	@Override
	public void initialise(IModel model, IView view) {
		
		this.model = model;
		this.view = view;
	}

	@Override
	public void startup() {
		// Initialise board
		model.clear(0);
		int width = model.getBoardWidth();
		int height = model.getBoardHeight();
		for ( int x = 0 ; x < width ; x++ ) {
			for ( int y = 0 ; y < height ; y++ ) {
				model.setBoardContents(x, y, 0);
			}
		}
		model.setFinished(false);
		/* setting initial pieces*/
		model.setBoardContents(3, 3, 1);
		model.setBoardContents(4, 4, 1);
		model.setBoardContents(3, 4, 2);
		model.setBoardContents(4, 3, 2);
		
		
		
		/*white should play first, even after a restart*/
		model.setPlayer(1);
		view.feedbackToUser(1, "White player - choose where to put your piece");
		view.feedbackToUser(2, "Black player - not your turn");
		
		/*refresh view*/
		view.refreshView();
		
		
	}

	@Override
	public void update() {
		
		/*boolean finished = true;
		for ( int x1 = 0 ; x1 < model.getBoardWidth() ; x1++ )
			for ( int y1 = 0 ; y1 < model.getBoardHeight() ; y1++ )
				if ( model.getBoardContents(x1, y1) == 0 )
					finished = false; 
		model.setFinished(finished);*/

		
		if(model.hasFinished() == true) {
			int whiteCount = 0;
			int blackCount = 0;
	        for (int j = 0; j <= 7; j++) {
	        	for (int k = 0; k <= 7; k++) {
	        		if(model.getBoardContents(j, k) == 1) {
	        			whiteCount++;
	        		}
	        		else if(model.getBoardContents(j, k) == 2) {
	        			blackCount++;
	        		}
	        	}
	        }
			if(whiteCount > blackCount) {
				view.feedbackToUser(1, "White won. White " +whiteCount+ " to Black " +blackCount + ". Reset the game to replay.");
				view.feedbackToUser(2, "White won. White " +whiteCount+ " to Black " +blackCount + ". Reset the game to replay.");
			}
			else if(whiteCount < blackCount) {
				view.feedbackToUser(1, "Black won. Black " +blackCount+ " to White " +whiteCount + ". Reset the game to replay.");
				view.feedbackToUser(2, "Black won. Black " +blackCount+ " to White " +whiteCount + ". Reset the game to replay.");
			}
			else if(whiteCount == blackCount) {
				view.feedbackToUser(1, "Draw. Both players ended with " +blackCount +" pieces. Reset the game to replay");
				view.feedbackToUser(2, "Draw. Both players ended with " +blackCount +" pieces. Reset the game to replay");
			}
		}
		if (model.hasFinished() == false) {
			
			
			
			if (whiteToPlay == true) {
    			model.setPlayer(1);
    			view.feedbackToUser(1, "White player - choose where to put your piece");
    			view.feedbackToUser(2, "Black player - not your turn");
    			

	        }
	        else if (blackToPlay == true) {
	        	model.setPlayer(2);
    			view.feedbackToUser(2, "Black player - choose where to put your piece");
    			view.feedbackToUser(1, "White player - not your turn");
    			
 
	        }
	        
	        
	        
		}


	}

	@Override
	public void squareSelected(int player, int x, int y) {
		
		/*check that the square selected is clicked by the player it should be*/
		if (player != model.getPlayer()) {
			if(player == 1) {
				view.feedbackToUser(1, "It is not your turn!");
			}
			else if (player == 2) {
				view.feedbackToUser(2, "It is not your turn!");
			}
		}
		else if (player == model.getPlayer()) {
			if(model.getBoardContents(x, y) == 0) {
				if(this.pieceCounter(x, y, player, 1) != 0) {
						
						
						
						/*if all checks pass then update board*/
						model.setBoardContents(x, y, player);
						view.refreshView();
						if (model.getPlayer() == 1) {
							/*if there is valid location to play then change to other player*/
							checkblack:
					        for (int j = 0; j < model.getBoardWidth(); j++) {
					        	for (int k = 0; k < model.getBoardHeight(); k++) {
					        		if (model.getBoardContents(j, k) == 0) {
						        		if(this.pieceCounter(j, k, 2, 0) != 0) {
						        			blackToPlay = true;
						        			whiteToPlay = false;
						        			break checkblack;
						        		}
						        		if(this.pieceCounter(j, k, 2, 0) == 0) {
						        			blackToPlay = false;
						        			if(this.pieceCounter(j, k, 1, 0) == 0)
						        				whiteToPlay = false;
						        		}
					        		}

					        	}
					        }
							


						}
						
						else if (model.getPlayer() == 2) {
							/*if there is valid location to play then change to other player*/
							checkwhite:
						        for (int j = 0; j < model.getBoardWidth(); j++) {
						        	for (int k = 0; k < model.getBoardHeight(); k++) {
						        		if (model.getBoardContents(j, k) == 0) {
							        		if(this.pieceCounter(j, k, 1, 0) != 0) {
							        			whiteToPlay = true;
							        			blackToPlay = false;
							        			break checkwhite;
							        		}
							        		if(this.pieceCounter(j, k, 1, 0) == 0) {
							        			whiteToPlay = false;
							        			if(this.pieceCounter(j, k, 2, 0) == 0)
							        				blackToPlay = false;
							        		}
						        		}
						        		

						        	}
						        }


						}
						

						
					}
			}
			boolean finished = true;
			for ( int x1 = 0 ; x1 < model.getBoardWidth() ; x1++ )
				for ( int y1 = 0 ; y1 < model.getBoardHeight() ; y1++ )
					if ( model.getBoardContents(x1, y1) == 0 )
						finished = false; 
			model.setFinished(finished);
			/*if neither player can move then end the game	*/
			if (whiteToPlay == false && blackToPlay == false) {
				model.setFinished(true);
				
			}
			this.update();
		}
		
		
		

		
		
		
				
		
	}

	@Override
	public void doAutomatedMove(int player) {
		int maxSeen = 0, result = 0,  autox = 0,  autoy = 0;
		for(int j = 0; j < model.getBoardWidth(); j++ ) {
			for(int k = 0; k < model.getBoardHeight(); k++) {
				if(model.getBoardContents(j, k) == 0) {
					result = this.pieceCounter(j, k, player, 0);
						if(result >= maxSeen) {
							maxSeen = result;
							autox = j;
							autoy = k;
						}
				}
			}
		}
		if(model.hasFinished() == false) {
			this.squareSelected(player, autox, autoy);
			view.refreshView();
		}
	}
	
	public int pieceCounter(int x, int y, int player, int doPaint) {
		
		int totalcount = 0, highestCount = 0 ;
		int xoffset, yoffset, whoOwnsSquare;
		
		
		
		for (xoffset=-1; xoffset<=1; xoffset++) {
			for (yoffset=-1; yoffset<=1; yoffset++) {
				if((x+xoffset > 0) && (x+xoffset < 8) && (y+yoffset > 0) && (y+yoffset < 8)) {
					whoOwnsSquare = model.getBoardContents(x+xoffset, y+yoffset);
					if ((whoOwnsSquare > 0) && (whoOwnsSquare != player)) {
						
						/* function to carry on going in offset direction*/
						totalcount = carryOn(xoffset, yoffset, x+xoffset, y+yoffset, player, doPaint);
						if (totalcount > highestCount) {
							highestCount = totalcount;
						}
					}

				}
			}
		}
		
		return highestCount;
	}
	
	
	public int carryOn (int xoffset, int yoffset, int x, int y, int player, int doPaint) {
		
		int count = 0, otherPlayer = 0, currentx = x, currenty = y, noToCapture = 0;
		boolean canPlay = false;
		
		if (player == 1){
			otherPlayer = 2;
		}
		else if (player == 2){
			otherPlayer = 1;
		}
		
		
		while(model.getBoardContents(currentx, currenty) == otherPlayer) {

			count++;
			currentx = currentx + xoffset;
			currenty = currenty + yoffset;
			if((currentx < 0) || (currentx >= 8) || (currenty < 0) || (currenty >= 8)) {
				break;
			}
			if(model.getBoardContents(currentx, currenty) == player) {
				canPlay = true;
				break;
			}
	
		}
	
		if (canPlay && doPaint == 1) {
			noToCapture = count+1;
			currentx = x;
			currenty = y;
			while (noToCapture > 0) {
				

				model.setBoardContents(currentx, currenty, player);
				currentx += xoffset;
				currenty += yoffset;
				
				
				
				noToCapture--;
			}
			
			return count+1;
		}
		else if (canPlay && doPaint == 0) {
			return count+1;
		}
		

		return 0;
		
	}

}
```
