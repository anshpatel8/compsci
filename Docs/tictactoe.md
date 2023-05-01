# Tic tac toe
## Controller
```
package tictac;

public class Controller {
	
	Model model;
	View view;
	
	public void initialise(Model model, View view) {
		this.model = model;
		this.view = view;
	}
	
	public void startup() {
		
		model.clear(0);
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
			if (this.checkEdges(x, y, player) == 1) {
				model.setFinished(true);
				view.feedbackToUser("White won");
			}
			else if (this.checkEdges(x, y, player) == 2) {
				model.setFinished(true);
				view.feedbackToUser("Black won");
			}
			
			else if (this.pieceCounter(x, y, player) == 3) {
				model.setFinished(true);
				if (player == 1) {
					view.feedbackToUser("White won");
				}
				else if (player == 2) {
					view.feedbackToUser("Black won");
				}
			}
			else {
				model.setPlayer(flipPlayer(player));
				if (player == 1) {
					view.feedbackToUser("Black player choose where to play");
				}
				else if (player == 2) {
					view.feedbackToUser("White player choose where to play");
				}
			}

		}
	}
	
	public int checkEdges(int x, int y, int player) {
		
		if ((x == 0) && (y == 1)) {
			if ((model.getBoardContents(0, 2) == player) && (model.getBoardContents(0, 0) == player)) {
				return player;
			}
		}
		if ((x == 1) && (y == 1)) {
			if ((model.getBoardContents(1, 2) == player) && (model.getBoardContents(1, 0) == player)) {
				return player;
			}
			if ((model.getBoardContents(0, 1) == player) && (model.getBoardContents(2, 1) == player)) {
				return player;
			}
		}
		if ((x == 2) && (y == 1)) {
			if ((model.getBoardContents(2, 2) == player) && (model.getBoardContents(2, 0) == player)) {
				return player;
			}
		}
		if ((x == 1) && (y == 0)) {
			if ((model.getBoardContents(0, 0) == player) && (model.getBoardContents(2, 0) == player)) {
				return player;
			}
		}
		if ((x == 1) && (y == 2)) {
			if ((model.getBoardContents(0, 2) == player) && (model.getBoardContents(2, 2) == player)) {
				return player;
			}
		}
		
		return 0;
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
	
	public int pieceCounter(int x, int y, int player) {
		
		int totalcount = 0;
		int xoffset, yoffset, whoOwnsSquare;
		
		for (xoffset=-1; xoffset<=1; xoffset++) {
			for (yoffset=-1; yoffset<=1; yoffset++) {
				if((x+xoffset > 0) && (x+xoffset < 3) && (y+yoffset > 0) && (y+yoffset < 3)) {
					whoOwnsSquare = model.getBoardContents(x+xoffset, y+yoffset);
					if ((xoffset == 0) && (yoffset == 0)) {
						continue;
					}
					if ((whoOwnsSquare > 0) && (whoOwnsSquare == player)) {
						/* function to carry on going in offset direction*/
						totalcount = carryOn(xoffset, yoffset, x+xoffset, y+yoffset, player);
						if (totalcount == 3) {
							return 3;
						}
					}

				}
			}
		}
		return 0;
	}
	
	public int carryOn (int xoffset, int yoffset, int x, int y, int player) {
		
		int count = 1, currentx = x, currenty = y;
		
		
		
		while(model.getBoardContents(currentx, currenty) == player) {

			count++;
			currentx = currentx + xoffset;
			currenty = currenty + yoffset;
			if((currentx < 0) || (currentx >= 3) || (currenty < 0) || (currenty >= 3)) {
				break;
			}
			if(count == 3) {
				break;
			}
	
		}
	

		

		return count;
		
	}

}

```
