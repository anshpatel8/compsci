# Tic tac toe
Made a tic tac toe game in Java using Swing
## Controller
```
package tictac;

public class Controller {
	
	Model model;
	View view;
	int count = 0;
	
	public void initialise(Model model, View view) {
		this.model = model;
		this.view = view;
	}
	
	public void startup() {
		
		model.clear(0);
		count = 0;
		int width = model.getBoardWidth();
		int height = model.getBoardHeight();
		for ( int x = 0 ; x < width ; x++ ) {
			for ( int y = 0 ; y < height ; y++ ) {
				model.setBoardContents(x, y, 0);
			}
		}
		model.setFinished(false);
		model.setPlayer(1);
		view.feedbackToUser("White player choose where to play");
		view.refreshView();
	}
	
	public void squareSelected(int player, int x, int y) {
		if(model.getBoardContents(x, y) == 0) {
			model.setBoardContents(x, y, player);
			view.refreshView();
			if (this.checkEdges(1) == true) {
				model.setFinished(true);
				view.feedbackToUser("White won");
			}
			else if (this.checkEdges(2) == true) {
				model.setFinished(true);
				view.feedbackToUser("Black won");
			}

			else if ((this.checkEdges(1) == false) && (this.checkEdges(2) == false) && (this.checkFull() == false)){
				model.setPlayer(flipPlayer(player));
				if (player == 1) {
					view.feedbackToUser("Black player choose where to play");
				}
				else if (player == 2) {
					view.feedbackToUser("White player choose where to play");
				}
			}
			else if ((this.checkEdges(1) == false) && (this.checkEdges(2) == false) && (this.checkFull() == true)) {
				model.setFinished(true);
				view.feedbackToUser("Draw");
			}
			
		}
	}
	
	public boolean checkFull() {
		
		count = 0;
		for(int i = 0; i <= 2; i++) {
			for(int j = 0; j <= 2; j++) {
				if(model.getBoardContents(i, j) == 0) {
					count++;
					//count is the number of empty squares
				}
			}
		}
		if(count == 0) {
			return true;
		}
		
		return false;
	}
	
	
	public boolean checkEdges(int player) {
		
		if ((model.getBoardContents(0, 0) == player) && (model.getBoardContents(0, 1) == player) && (model.getBoardContents(0, 2) == player)){
			return true;
		}
		if ((model.getBoardContents(1, 0) == player) && (model.getBoardContents(1, 1) == player) && (model.getBoardContents(1, 2) == player)){
			return true;
		}
		if ((model.getBoardContents(2, 0) == player) && (model.getBoardContents(2, 1) == player) && (model.getBoardContents(2, 2) == player)){
			return true;
		}
		if ((model.getBoardContents(0, 0) == player) && (model.getBoardContents(1, 0) == player) && (model.getBoardContents(2, 0) == player)){
			return true;
		}
		if ((model.getBoardContents(0, 1) == player) && (model.getBoardContents(1, 1) == player) && (model.getBoardContents(2, 1) == player)){
			return true;
		}
		if ((model.getBoardContents(0, 2) == player) && (model.getBoardContents(1, 2) == player) && (model.getBoardContents(2, 2) == player)){
			return true;
		}
		if ((model.getBoardContents(0, 0) == player) && (model.getBoardContents(1, 1) == player) && (model.getBoardContents(2, 2) == player)){
			return true;
		}
		if ((model.getBoardContents(2, 0) == player) && (model.getBoardContents(1, 1) == player) && (model.getBoardContents(0, 2) == player)){
			return true;
		}
		
		return false;
	}
	
	public int flipPlayer(int player) {
		
		if (player == 1) {
			return 2;
		}
		else if (player == 2) {
			return 1;
		}
		return 0;
	}
}
```
## View
```
package tictac;

import java.awt.BorderLayout;
import java.awt.Color;
import java.awt.GridLayout;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;

import javax.swing.JButton;
import javax.swing.JFrame;
import javax.swing.JLabel;
import javax.swing.JPanel;




public class View {

	Model model;
	Controller controller;
	
	//constructor
	public View() 
	{
	}
	
	//frame creation
	JFrame playerframe = new JFrame();
	JLabel playerlabel = new JLabel();
	JPanel playergrid = new JPanel(new GridLayout(3,3));
	JPanel buttons = new JPanel(new GridLayout(1,1));
	ticsquare[][] square = new ticsquare[3][3];
	
	public void initialise(Model model, Controller controller) {
		
		this.model = model;
		this.controller = controller;
		
		playerframe.setLayout(new BorderLayout());
		playerframe.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
		playerframe.setTitle("Tic-Tac-Toe");
		playerframe.setLocationRelativeTo(null);
		playerlabel.setText("Tic-Tac-Toe");
		playerframe.add(playerlabel, BorderLayout.NORTH);
		playerframe.add(playergrid, BorderLayout.CENTER);
        for (int x = 0; x <= 2; x++) {
        	for (int y = 0; y <= 2; y++) {
        		ticsquare button = new ticsquare(x,y, model, controller);
        		square[x][y] = button;
        		playergrid.add(button);
        	}
            
        }
        playerframe.add(buttons, BorderLayout.SOUTH);
        JButton restart = new JButton();
        restart.setText("Restart");
        restart.addActionListener(new ActionListener()
    	{ public void actionPerformed(ActionEvent e) { controller.startup(); } } );
        buttons.add(restart);
		
		playerframe.pack();
		playerframe.setVisible(true);
	}
	
	public void refreshView() {
		
		for(int x = 0; x <= 2 ; x++) {
			for(int y = 0; y <= 2; y++) {
				int check = model.getBoardContents(x, y);
				if (check == 1) {
					square[x][y].setDrawColor(Color.BLACK);
					square[x][y].setFillColor(Color.WHITE);
					square[x][y].repaint();
				}
				else if (check == 2) {
					square[x][y].setDrawColor(Color.WHITE);
					square[x][y].setFillColor(Color.BLACK);
					square[x][y].repaint();
				}
				else if (check == 0) {
					square[x][y].setDrawColor(Color.GREEN);
					square[x][y].setFillColor(Color.GREEN);
					square[x][y].repaint();
				}
				
			}
		}
	}
	
	public void feedbackToUser(String message) {
		
		playerlabel.setText(message);
	}
	
}
```
## Model
```
package tictac;


public class Model {

	int [][] boardContents;
	int width;
	int height;
	int player;
	boolean finished;
	View view;
	Controller controller;
	
	
	public void initialise(int width, int height, View view, Controller controller)
	{
		this.width = width;
		this.height = width;
		this.view = view;
		this.controller = controller;
		boardContents = new int[width][height];
	}

	
	public void clear(int value)
	{
		for ( int x = 0 ; x < width ; x++ )
			for ( int y = 0 ; y < width ; y++ )
				boardContents[x][y] = value;
	}

	
	public int getBoardWidth()
	{
		return width;
	}

	
	public int getBoardHeight()
	{
		return height;
	}

	
	public int getBoardContents(int x, int y)
	{
		return boardContents[x][y];
	}

	
	public void setBoardContents(int x, int y, int value)
	{
		boardContents[x][y] = value;
	}

	
	public void setPlayer(int player)
	{
		this.player = player;
	}

	
	public int getPlayer()
	{
		return player;
	}

	
	public boolean hasFinished()
	{
		return finished;
	}

	
	public void setFinished(boolean finished)
	{
		this.finished = finished;
	}

}
```
## Main
```
package tictac;

public class TictacMain {

	Model model;
	View view;
	Controller controller;
	
	TictacMain()
	{
		model = new Model();
		view = new View();
		controller = new Controller();
		
		//Initialise everything
		model.initialise(3, 3, view, controller);
		view.initialise(model, controller);
		controller.initialise(model, view);
		
		//Start game
		controller.startup();
	}
	
	public static void main(String[] args)
	{
		new TictacMain();
	}
}
```
## ticsquare
```
package tictac;

import java.awt.Color;
import java.awt.Dimension;
import java.awt.Graphics;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;

import javax.swing.BorderFactory;
import javax.swing.JButton;

public class ticsquare extends JButton{

	Model model;
	Controller controller;
	
	Color drawColor;
	Color fillColor;
	
	public ticsquare(int x, int y, Model model, Controller controller) {
		
		this.model = model;
		this.controller = controller;
		
		
		setBackground(Color.GREEN);
		setPreferredSize(new Dimension(120, 120));	
		setBorder(BorderFactory.createLineBorder(Color.BLACK));
		addActionListener(new ActionListener(){
			public void actionPerformed(ActionEvent e) {
				if(model.hasFinished() == false) {
					controller.squareSelected(model.getPlayer(), x, y);
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
	protected void paintComponent(Graphics g){
		super.paintComponent(g);

			if(drawColor != null) {
				g.setColor(drawColor);
				g.drawOval(3, 3, 114, 114);
			}
			if(fillColor != null) {
				g.setColor(fillColor);
				g.fillOval(3, 3, 114, 114);
			}
	}
}
```
