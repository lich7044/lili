

 import java.util.*;
public class li{
   public static void main(String argc[]){
      ExprTree et = new ExprTree();
      et.inToTree("(3+(4*5))");
      et.print(); //print above
      et.inToTree("((1-(2+3))+(4*5))");
      et.print(); //print above
      et.inToTree("(3(4+5))");
      et.print(); //no output
      et.inToTree("((4+5)*2+1)");
      et.print(); //no output
      et.inToTree("((4+5)*3))");
      et.print(); //no output
      et.postToTree("34+5");
      et.print(); //no output
/*
      System.out.println("Enter an algebraic expression: ");
      Scanner s = new Scanner(System.in);
      String alg =  s.nextLine();
      s.close();
      et = new ExprTree(alg);
      et.print();
*/
      Tree t1 = new Tree();
      Tree t2 = new Tree();
      t1.makeTree();
      t1.levelOrder();	//0 1 2 3 4 5 6 13 14 15 7 8 9 10 11 12
      t2.makeTree2();
      t2.levelOrder();	//0 1 2 3
/*    System.out.println("sub tree t1 and t1 " + t1.isSubTree(t1));
      System.out.println("sub tree t1 and t2 " + t1.isSubTree(t2)); //t2 is not a subTree of t1
      Tree t3 = new Tree();
      t3.makeTree3();
      t3.levelOrder();
      System.out.println("sub tree t1 and t3 " + t1.isSubTree(t3)); //t3 is a subTree of t1
      Tree t4 = new Tree();
      t4.makeTree4();
      t4.levelOrder();
      System.out.println("sub tree t1 and t4 " + t1.isSubTree(t4)); //t4 is a subTree of t1
*/
   }
}
      
class ExprTree{
   private static class BTNode{
      char value;
      BTNode parent, left, right;
      public BTNode(char e){
        this(e, null, null, null);
      }
      public BTNode(char e, BTNode p, BTNode l, BTNode r){
	value = e;
	parent = p;
	left = l;
	right = r;
      }
   }
   BTNode root;
   public ExprTree(){
      root = null;
   }
   public void postToTree(String exp){
	      
   }
public boolean isEmpty(){
      return root == null;
   }
   public void inToTree(String expression){
         root = parseInToTree(expression);
   }

   public BTNode parseInToTree(String a){
	      //if (a.length() == 1)
	    	 if (Character.isDigit(a.charAt(0))) return new BTNode(a.charAt(0));
	      if (a.length() >=3 && a.charAt(0) != '(' ||a.charAt(a.length() - 1) == ')')
	      a = a.substring(1, a.length() - 1);
	     
	      String op = "+-*";
	    
	      Deque<Character> stack = new ArrayDeque<Character>();
	      int ansidx =-1,i=0;
	      while(i<a.length()) {
	    	  char c = a.charAt(i);
		         if (c == '(') stack.addLast(c);
				 else if (c  == ')')
				    if (!stack.isEmpty())  stack.removeLast();
				    else return null;
				    //else stack.removeLast();
				 else if (stack.isEmpty() && op.contains(Character.toString(c)))
				    if (ansidx <=-1) ansidx = i;
				    else return null;
		      i++;
	      }
	      if (ansidx <= 0 || ansidx == a.length() - 1) return null;
	      BTNode l = parseInToTree(a.substring(0, ansidx));
	      BTNode r = parseInToTree(a.substring(ansidx + 1));
	      if (l== null ) return null;
	      if( r == null)return null;
	      BTNode ans = new BTNode(a.charAt(ansidx),null,l,r);
	      l.parent = r.parent = ans;
	      return ans;
	   }

 public boolean isLeaf(BTNode n){ 
	 while(n != null && n.left == null && n.right == null) {
	 return true;
	}
		 return false;
 }

 
 public void print(){
	   infix(root);
	  System.out.println();
 }
 
 
 
 
 public void infix(BTNode n){
     if (n == null) return;
     if (isLeaf(n)){
	 System.out.print(n.value);
	 return;
     }
     System.out.print("(");
     infix(n.left);
     System.out.print(n.value);
     infix(n.right);
     System.out.print(")");
  }

}
class Tree{
	private static class TNode{
	      private int value;
	      private TNode parent;
	      private List<TNode> children;
	      public TNode(){
	         this(0, null);
	      }
	      public TNode(int e){
	         this(e, null);
	      }
	      public TNode(int e, TNode p){
	         value = e;
	         parent = p;
	         children = new ArrayList<TNode>();
	      }
		public int getValue() {
			 
			return value;
		}
		
		public List<TNode> getChildren(){
	        return children;
	     }
		
	   }
	   private TNode root;
	   private int size;
	   Tree(){
	      root = null;
	      size = 0;
	   }
	   
	   public void levelOrder() {
	    	  Queue<TNode> queue = new ArrayDeque<>();
	    		//create queue, add root to queue
	    	      if (root != null) queue.add(root);
	    	      levelOrderPrint(queue);
	    	      System.out.println();
		
	}
	    private void levelOrderPrint(Queue<TNode> q){
	        if (q.isEmpty()) return;
	        TNode temp = q.remove();
	        System.out.print(temp.getValue() + " ");
	        for (TNode cn: temp.getChildren()){
	           q.add(cn);
	       }
	       levelOrderPrint(q);
	     }
	   
	  /** public  void levelOrder() {
			TNode root = queue.poll();
			if (root== null) {
				return;
			}
			root.display();
			if (root.hasLeftChildren()) {
				queue.add(root.leftchildren);
			}
			if (root.hasRightchildren()) {
				queue.add(root.rightchildren);
			}
			levelOrder(queue);
		}
	**/    
	   
	    
	    public TNode createNode(int e, TNode p){
	       return new TNode(e, p);
	    }
	    public TNode addChild(TNode n, int e){
	       TNode temp = createNode(e, n);
	       n.children.add(temp);
	       size++;
	       return temp;
	    }
	    public void makeTree(){
	       root = createNode(0, null);
	       size++;
	       buildTree(root, 3);
	    }
	    public void makeTree2(){
	       root = createNode(0, null); 
	       size++; 
	       buildTree(root, 1);
	    }
	    public void makeTree3(){
	       root = createNode(3, null); 
	       size++; 
	    }
	    public void makeTree4(){
	       root = createNode(2, null); 
	       size += 13; 
	       buildTree(root, 1);
	       size -= 12;
	    }
	    private void buildTree(TNode n, int i){
	       if (i <= 0) return;
	       TNode fc = addChild(n, size);
	       TNode sc = addChild(n, size);
	       TNode tc = addChild(n, size);
	       buildTree(fc, i - 1);
	       buildTree(sc, i - 2);
	       if (i % 2 == 0)
	          buildTree(tc, i - 1);
	   }
	}   
