
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Backtracking</title>
    
</head>
<body>
    <h1 id="back">Backtracking</h1>
    <p class="copy">Copyright ©2002 by David Matuszek</p>
    <p>Backtracking is a form of recursion.</p>
    <div>
    <p>The usual scenario is that you are faced with a number of options, and you
        must choose one of these. After you make your choice you will get a new set
        of options; just what set of options you get depends on what choice you made.
        This procedure is repeated over and over until you reach a final state. If you
        made a good sequence of choices, your final state is a <i>goal state;</i> if
        you didn't, it isn't.</p>
    <p>Conceptually, you start at the root of a tree; the tree probably has some good
        leaves and some bad leaves, though it may be that the leaves are all good or
        all bad. You want to get to a good leaf. At each node, beginning with the root,
        you choose one of its children to move to, and you keep this up until you get
        to a leaf.</p>
    <p>Suppose you get to a bad leaf. You can <i>backtrack</i> to continue the search
        for a good leaf by revoking your <i>most recent</i> choice, and trying out the
        next option in that set of options. If you run out of options, revoke the choice
        that got you here, and try another choice at that node. If you end up at the
        root with no options left, there are no good leaves to be found.</p>
    <p>This needs an example.</p>
    </div>
    <center>
    <img src="https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcRrTDkJ9wYuhCMfNCwWbQ6vqUOZkTncrFpMoQ&usqp=CAU" alt="tree"></center>
    <ol>
        <li>Starting at Root, your options are A and B. You choose A.</li>
        <li>At A, your options are C and D. You choose C.</li>
        <li>C is bad. Go back to A.</li>
        <li>At A, you have already tried C, and it failed. Try D.</li>
        <li>D is bad. Go back to A.</li>
        <li>At A, you have no options left to try. Go back to Root.</li>
        <li>At Root, you have already tried A. Try B.</li>
        <li>At B, your options are E and F. Try E.</li>
        <li>E is good. Congratulations!</li>
    </ol>
    <p>In this example we drew a picture of a tree. The tree is an abstract model
        of the possible sequences of choices we could make. There is also a data structure
        called a tree, but usually we don't have a data structure to tell us what choices
        we have. (If we do have an actual tree data structure, backtracking on it is
        called <i>depth-first tree searching.</i>)</p>
    <p><b> <font size="+1"> The backtracking algorithm.</font></b></p>
    <p>Here is the algorithm (in pseudocode) for doing backtracking from a given node
        n:</p>
    <blockquote>
        <pre>boolean solve(Node n) {
    if n is a leaf node {
        if the leaf is a goal node, return true
        else return false
    } else {
        for each child c of n {
            if solve(c) succeeds, return true
        }
        return false
    }
}</pre>
    </blockquote>
    <p>Notice that the algorithm is expressed as a <i>boolean</i> function. This is
        essential to understanding the algorithm. If <code>solve(n)</code> is true,
        that means node<code> n </code>is part of a solution--that is, node<code> n 
  </code>is one of the nodes on a path from the root to some goal node. We say
        that<code> n </code>is <i>solvable</i>. If<code> solve(n) </code>is false, then
        there is <i>no</i> path that includes<code> n</code> to any goal node.</p>
    <p>How does this work?</p>
    <ul>
        <li>If any child of<code> n </code>is solvable, then<code> n </code>is solvable.</li>
        <li>If no child of<code> n </code>is solvable, then<code> n </code>is not solvable.</li>
    </ul>
    <p>Hence, to decide whether any non-leaf node<code> n </code>is solvable (part
        of a path to a goal node), all you have to do is test whether any child of<code>
  n </code>is solvable. This is done recursively, on each child of<code> n</code>.
        In the above code, this is done by the lines</p>
    <pre>        for each child c of n {
            if solve(c) succeeds, return true
        }
        return false</pre>
    <p>Eventually the recursion will "bottom" out at a leaf node. If the
        leaf node is a goal node, it is solvable; if the leaf node is not a goal node,
        it is not solvable. This is our base case. In the above code, this is done by
        the lines</p>
    <blockquote>
        <pre>if n is a leaf node {
    if the leaf is a goal node, return true
    else return false
}</pre>
    </blockquote>
    <p>The backtracking algorithm is simple but important. You should understand it
        thoroughly. Another way of stating it is as follows:</p>
    <dir>
        <li>To search a tree:
            <ol>
                <li> If the tree consists of a single leaf, test whether it is a goal node,</li>
                <li>Otherwise, search the subtrees until you find one containing a goal
                    node, or until you have searched them all unsuccessfully.</li>
            </ol>
        </li>
    </dir>
    <p><b>
            <font size="+1"><br />
                Non-recursive backtracking, using a stack</font>
        </b></p>
    <p>Backtracking is a rather typical recursive algorithm, and any recursive algorithm
        can be rewritten as a stack algorithm. In fact, that is how your recursive algorithms
        are translated into machine or assembly language.</p>
    <blockquote>
        <pre>boolean solve(Node n) {
    put node n on the stack;
    while the stack is not empty {
        if the node at the top of the stack is a leaf {
            if it is a goal node, return true
            else pop it off the stack
        }
        else {
            if the node at the top of the stack has untried children
                push the next untried child onto the stack
            else pop the node off the stack

    }
    return false
}</pre>
    </blockquote>
    <p>Starting from the root, the only nodes that can be pushed onto the stack are
        the children of the node currently on the top of the stack, and these are only
        pushed on one child at a time; hence, the nodes on the stack at all times describe
        a valid path in the tree. Nodes are removed from the stack only when it is known
        that they have no goal nodes among their descendents. Therefore, if the root
        node gets removed (making the stack empty), there must have been no goal nodes
        at all, and no solution to the problem.</p>
    <p>When the stack algorithm terminates successfully, the nodes on the stack form
        (in reverse order) a path from the root to a goal node.</p>
    <p>Similarly, when the recursive algorithm finds a goal node, the path information
        is embodied (in reverse order) in the sequence of recursive calls. Thus as the
        recursion unwinds, the path can be recovered one node at a time, by (for instance)
        printing the node at the current level, or storing it in an array.</p>
    <p>Here is the recursive backtracking algorithm, modified slightly to print (in
        reverse order) the nodes along the successful path:</p>
    <blockquote>
        <pre>boolean solve(Node n) {
    if n is a leaf node {
        if the leaf is a goal node {
           print n
           return true
        }
        else return false
    } else {
        for each child c of n {
            if solve(c) succeeds {
                print n
                return true
            }
        }
        return false
    }
}</pre>
    </blockquote>
    <p><b>
            <font size="+1"><br />
                Keeping backtracking simple</font>
        </b></p>
    <p>All of these versions of the backtracking algorithm are pretty simple, but
        when applied to a real problem, they can get pretty cluttered up with details.
        Even determining whether the node is a leaf can be complex: for example, if
        the path represents a series of moves in a chess endgame problem, the leaves
        are the checkmate and stalemate solutions. </p>
    <p>To keep the program clean, therefore, tests like this should be buried in methods.
        In a chess game, for example, you could test whether a node is a leaf by writing
        a<code> gameOver </code>method (or you could even call it<code> isLeaf</code>).
        This method would encapsulate all the ugly details of figuring out whether any
        possible moves remain.</p>
    <p>Notice that the backtracking altorithms require us to keep track, for each
        node on the current path, which of its children have been tried already (so
        we don't have to try them again). In the above code we made this look simple,
        by just saying <code>for&nbsp;each&nbsp;child&nbsp;c&nbsp;of&nbsp;n</code>.
        In reality, it may be difficult to figure out what the possible children are,
        and there may be no obvious way to step through them. In chess, for example,
        a node can represent one arrangement of pieces on a chessboard, and each child
        of that node can represent the arrangement after some piece has made a legal
        move. How do you find these children, and how do you keep track of which ones
        you've already examined?</p>
    <p>The most straightforward way to keep track of which children of the node have
        been tried is as follows: Upon initial entry to the node (that is, when you
        first get there from above), make a list of all its children. As you try each
        child, take it off the list. When the list is empty, there are no remaining
        untried children, and you can return "failure." This is a simple approach,
        but it may require quite a lot of additional work.</p>
    <p>There is an easier way to keep track of which children have been tried, <b>if</b>
        you can define an ordering on the children. If there is an ordering, and you
        know which child you just tried, you can determine which child to try next.</p>
    <p>For example, you might be able to number the children<code> 1</code> through<code>
  n</code>, and try them in numerical order. Then, if you have just tried child<code>
  k</code>, you know that you have already tried children<code> 1 </code>through<code>
  k-1</code>, and you have not yet tried children<code> k+1</code> through<code>
  n</code>. Or, if you are trying to color a map with just four colors, you can
        always try red first, then yellow, then green, then blue. If child yellow fails,
        you know to try child green next. If you are searching a maze, you can try choices
        in the order left, straight, right (or perhaps north, east, south, west).</p>
    <p>It isn't always easy to find a simple way to order the children of a node.
        In the chess game example, you might number your pieces (or perhaps the squares
        of the board) and try them in numerical order; but in addition each piece may
        also have several moves, and these must also be ordered.</p>
    <p>You can probably find some way to order the children of a node. If the ordering
        scheme is simple enough, you should use it; but if it is too cumbersome, you
        are better off keeping a list of untried children.</p>
    <p><b>
            <font size="+1"><br />
                Example: TreeSearch</font>
        </b></p>
    <p>For starters, let's do the simplest possible example of backtracking, which
        is searching an actual tree. We will also use the simplest kind of tree, a binary
        tree.</p>
    <p>A <b>binary tree </b>is a data structure composed of <b>nodes</b>. One node
        is designated as the <b>root node</b>. Each node can reference (point to) zero,
        one, or two other nodes, which are called its <b>children</b>. The children
        are referred to as the <b>left child</b> and/or the <b>right child</b>. All
        nodes are reachable (by one or more steps) from the root node, and there are
        no cycles. For our purposes, although this is not part of the definition of
        a binary tree, we will say that a node might or might not be a goal node, and
        will contain its name. The first example in this paper (which we repeat here)
        shows a binary tree.</p>
   
    
    <p>Here's a definition of the BinaryTree class:</p>
    <blockquote>
        <pre>public class BinaryTree {
    BinaryTree leftChild = null;
    BinaryTree rightChild = null;
    boolean isGoalNode = false;
    String name;
    
    BinaryTree(String name, BinaryTree left, BinaryTree right, boolean isGoalNode) {
        this.name = name;
        leftChild = left;
        rightChild = right;
        this.isGoalNode = isGoalNode;
    }
}</pre>
    </blockquote>

    <p>Next we will create a <code>TreeSearch</code> class, and in it we will define
        a method<code> makeTree() </code>which constructs the above binary tree. </p>
    <blockquote>
        <pre>static BinaryTree makeTree() {
    BinaryTree root, a, b, c, d, e, f;
    c = new BinaryTree("C", null, null, false);
    d = new BinaryTree("D", null, null, false);
    e = new BinaryTree("E", null, null, true);
    f = new BinaryTree("F", null, null, false);
    a = new BinaryTree("A", c, d, false);
    b = new BinaryTree("B", e, f, false);
    root = new BinaryTree("Root", a, b, false);
    return root;
}</pre>
    </blockquote>
    <p>Here's a main program to create a binary tree and try to solve it:</p>
    <blockquote>
        <pre>public static void main(String args[]) {
    BinaryTree tree = makeTree();
    System.out.println(solvable(tree));
}</pre>
    </blockquote>
    <p>And finally, here's the recursive backtracking routine to "solve"
        the binary tree by finding a goal node.</p>
    <blockquote>
        <pre>static boolean solvable(BinaryTree node) {
<font color="#0000ff">/* 1 */</font>  if (node == null) return false;
<font color="#0000ff">/* 2 */</font>  if (node.isGoalNode) return true;
<font color="#0000ff">/* 3 */</font>  if (solvable(node.leftChild)) return true;
<font color="#0000ff">/* 4 */</font>  if (solvable(node.rightChild)) return true;
<font color="#0000ff">/* 5 */</font>  return false;
}</pre>
    </blockquote>
    <p>Here's what the numbered lines are doing:</p>
    <ol>
        <li>If we are given a null node, it's not solvable. This statement is so that
            we can call this method with the children of a node, without first checking
            whether those children actually exist.</li>
        <li>If the node we are given is a goal node, return success.</li>
        <li>See if the left child of <code>node</code> is solvable, and if so, conclude
            that <code>node</code> is solvable. We will only get to this line if <code>node</code>
            is non-null and is not a goal node, says to </li>
        <li>Do the same thing for the right child.</li>
        <li>Since neither child of <code>node</code> is solvable, <code>node</code>
            itself is not solvable.</li>
    </ol>
    <p>This program runs correctly and produces the unenlightening result <code>true</code>.</p>
    <p> Each time we ask for another node, we have to check if it is <code>null</code>.
        In the above we put that check as the first thing in <code>solvable</code>.
        An alternative would be to check first whether each child exists, and recur
        only if they do. Here's that alternative version: </p>
    <blockquote>
        <pre>static boolean solvable(BinaryTree node) {
    if (node.isGoalNode) return true;
    if (node.leftChild != null &amp;&amp; solvable(node.leftChild)) return true;
    if (node.rightChild != null &amp;&amp; solvable(node.rightChild)) return true;
    return false;
}</pre>
    </blockquote>
    <p>I think the first version is simpler, but the second version is slightly more
        efficient. </p>
    <p><b>
            <font size="+1"><br />
                What are the children?</font>
        </b></p>
    <p>One of the things that simplifies the above binary tree search is that, at
        each choice point, you can ignore all the previous choices. Previous choices
        don't give you any information about what you should do next; as far as you
        know, both the left and the right child are possible solutions. In many problems,
        however, you may be able to eliminate children immediately, without recursion.</p>
    <p>Consider, for example, the problem of four-coloring a map. It is a theorem
        of mathematics that any map on a plane, no matter how convoluted the countries
        are, can be colored with at most four colors, so that no two countries that
        share a border are the same color.</p>
    <p>To color a map, you choose a color for the first country, then a color for
        the second country, and so on, until all countries are colored. There are two
        ways to do this:</p>
    <ul>
        <li><b>Method 1. </b>Try each of the four possible colors, and recur. When you
            run out of countries, check whether you are at a goal node.</li>
        <li><b>Method 2. </b>Try only those colors that have not already been used for
            an adjacent country, and recur. If and when you run out of countries, you
            have successfully colored the map.</li>
    </ul>
    <p>Let's apply each of these two methods to the problem of coloring a checkerboard.
        This should be easily solvable; after all, a checkerboard only needs two colors.</p>
    <p>In both methods, the colors are represented by integers, from<code> RED=1 </code>to<code>
  BLUE=4</code>. We define the following helper methods. The helper method code
        isn't displayed here because it's not important for understanding the method
        that does the backtracking.</p>
    <dl>
        <dt><code>boolean mapIsOK()</code></dt>
        <dd>Used by method 1 to check (at a leaf node) whether the entire map is colored
            correctly.
            <br /><br />
        </dd>
        <dt><code>boolean okToColor(int row, int column, int color)</code></dt>
        <dd>Used by method 2 to check, at every node, whether there is an adjacent node
            already colored with the given color.
            <br /><br />
        </dd>
        <dt><code>int[] nextRowAndColumn(int row, int column)</code></dt>
        <dd>Used by both methods to find the next "country" (actually, the
            row and column of the next square on the checkerboard).</dd>
    </dl>
    <p>Here's the code for method 1:</p>
    <blockquote>
        <pre>boolean explore1(int row, int column, int color) {
    if (row &gt;= NUM_ROWS) <b>return mapIsOK()</b>;
    map[row][column] = color;
    for (int nextColor = RED; nextColor &lt;= BLUE; nextColor++) {
        int[] next = nextRowAndColumn(row, column);
        if (explore1(next[0], next[1], nextColor)) return true;
    }
    return false;
}</pre>
    </blockquote>
    <p>And here's the code for method 2:</p>
    <blockquote>
        <pre>boolean explore2(int row, int column, int color) {
    if (row &gt;= NUM_ROWS) <b>return true</b>;
    <b>if (okToColor(row, column, color)) {</b>
        map[row][column] = color;
        for (int nextColor = RED; nextColor &lt;= BLUE; nextColor++) {
            int[] next = nextRowAndColumn(row, column);
            if (explore2(next[0], next[1], nextColor)) return true;
        }
    }
    return false;
}</pre>
    </blockquote>
    <p>Those appear pretty similar, and you might think they are equally good. However,
        the timing information suggests otherwise:</p>
    <table align="center" border="1" cellpadding="4">
        <tbody>
            <tr bgcolor="#cccccc">
                <td bgcolor="#ffffff">&nbsp;</td>
                <td>2 by 3 map</td>
                <td>3 by 3 map</td>
                <td>3 by 4 map</td>
            </tr>
            <tr>
                <td bgcolor="#cccccc">Method 1:</td>
                <td>60 ms.</td>
                <td>940 ms.</td>
                <td>60530 ms. (1 minute)</td>
            </tr>
            <tr>
                <td bgcolor="#cccccc">Method 2:</td>
                <td>0ms.</td>
                <td>0 ms.</td>
                <td>0 ms</td>
            </tr>
        </tbody>
    </table>
    <p>The zeros in the above table indicate times too short to measure (less than
        1 millisecond). Why this huge difference? Either of these methods <i>could</i>
        have exponential growth. Eliminating a node automatically eliminates all of
        its descendents, and this will often prevent exponential growth. Conversely,
        by waiting to check until a leaf node is reached, exponential growth is practically
        guaranteed. <i> <b>If there is any way to eliminate children (reduce the set
                of choices), do so!</b></i></p>
    <p>
        <font size="+1"><b><br />
                Debugging techniques</b></font>
    </p>
    <p> Often our first try at a program doesn't work, and we need to debug it. Debuggers
        are helpful, but sometimes we need to fall back on inserting print statements.
        There are some simple tricks to making effective use of print statements. These
        tricks can be applied to any program, but are especially useful when you are
        trying to debug recursive routines.</p>
    <p><b>Trick #1: Indent when you print method entries and exits. </b>Often, the
        best debugging technique is to print every method call and return (or at least
        the most important ones). You probably want to print, for each method, what
        parameters it came in with, and what value it leaves with. However, if you just
        print a long list of these, it's hard to match up method exits with their corresponding
        entries. Indenting to show the level of nesting can help.</p>
    <p><b>Trick #2: Use specialized print methods for debugging. </b>Don't clutter
        up your actual code more than you must. Also, remember that code inserted for
        debugging purposes can itself contain bugs, or (in the worst case) can affect
        the results, so be very careful with it.</p>
    <p>Here's our debugging code. For this trivial program, there's almost more debugging
        code than actual code, but in larger programs the proportions will be better.</p>
    <blockquote>
        <pre>static String indent = "";

static String name(BinaryTree node) {
    if (node == null) return null;
    else return node.name;
}

static void enter(BinaryTree node) {
    System.out.println(indent + "Entering solvable(" + name(node) + ")");
    indent = indent + "|  ";
}

static boolean yes(BinaryTree node) {
    indent = indent.substring(3);
    System.out.println(indent + "solvable(" + name(node) + ") returns true");
    return true;
}

static boolean no(BinaryTree node) {
    indent = indent.substring(3);
    System.out.println(indent + "solvable(" + name(node) + ") returns false");
    return false;
}</pre>
    </blockquote>
    <p>To use this code, we modify solvable as follows:</p>
    <blockquote>
        <pre>static boolean solvable(BinaryTree node) {
    <b>enter(node)</b>;
    if (node == null) return <b>no(node)</b>;
    if (node.isGoalNode) return <b>yes(node)</b>;
    if (solvable(node.leftChild)) return <b>yes(node)</b>;
    if (solvable(node.rightChild)) return <b>yes(node)</b>;
    return <b>no(node)</b>;
}</pre>
    </blockquote>
    <p>And we get these results:</p>
    <blockquote>
        <pre><font color="#000000"><b>Entering solvable(Root)
|  Entering solvable(A)
|  |  Entering solvable(C)
|  |  |  Entering solvable(null)
|  |  |  solvable(null) returns false
|  |  |  Entering solvable(null)
|  |  |  solvable(null) returns false
|  |  solvable(C) returns false
|  |  Entering solvable(D)
|  |  |  Entering solvable(null)
|  |  |  solvable(null) returns false
|  |  |  Entering solvable(null)
|  |  |  solvable(null) returns false
|  |  solvable(D) returns false
|  solvable(A) returns false
|  Entering solvable(B)
|  |  Entering solvable(E)
|  |  solvable(E) returns true
|  solvable(B) returns true
solvable(Root) returns true
true</b></font></pre>
    </blockquote>
    <p><b>Trick #3: Never discard your debugging statements. </b>Writing debugging
        statements is programming, too. Often it's as much work to debug the debugging
        statements as it is to debug the actual program. Once your program is working,
        why throw this code away?</p>
    <p>Obviously, you don't want to print out all this debugging information from
        a program you are ready to submit (or to turn over to your manager). You could
        comment out your debugging calls, but that can be a lot of work. What's more,
        in the above example, you would have to replace every <code>return(yes(node)) 
  </code>with<code> return(true)</code>, and every<code> return(no(node))</code>
        with<code> return false</code>. With all these changes, you might introduce
        new bugs into your program.</p>
    <p>The simple solution is to make your debugging statements <i>conditional.</i>
        For example,</p>
    <blockquote>
        <pre><b>static final boolean debugging = false;</b>

static void enter(BinaryTree node) {
    <b>if (debugging) {</b>
        System.out.println(indent + "Entering solvable(" + name(node) + ")");
        indent = indent + "|  ";
    <b>}</b>
}

static boolean yes(BinaryTree node) {
    <b>if (debugging) {</b>
        indent = indent.substring(3);
        System.out.println(indent + "solvable(" + name(node) + ") returns true");
    <b>}</b>
    return true;
}

static boolean no(BinaryTree node) {
   <b> if (debugging) {</b>
        indent = indent.substring(3);
        System.out.println(indent + "solvable(" + name(node) + ") returns false");
    <b>}</b>
    return false;
}</pre>
    </blockquote>
    In industry, actual programs often have multiple flags to control different aspects
    of debugging. Don't worry too much about making your code larger; modern compilers
    will notice that since the variable <code>debugging</code> is <code>final</code>,
    it can never be <code>true</code>, and the controlled code will be discarded.
    <p><b>Trick #4: Create an Exception. </b>If an Exception is thrown, you can get
        information about just where it happened by sending it the message<code> printStackTrace(PrintStream)</code>.
        Since an Exception is an object like any other, you can create and <code>throw</code>
        your own Exceptions. However, Java programmers don't always realize that you
        can create an Exception <i>without</i> throwing it. For example, the following
        code</p>
    <blockquote>
        <pre>new Exception("Checkpoint Charlie").printStackTrace(System.out);</pre>
    </blockquote>
    <p>will print out a message something like this, and the program will then continue
        normally. That is, the above code just acts like a print statement.</p>
    <blockquote>
        <pre>java.lang.Exception: Checkpoint Charlie
        at TreeSearch.solvable(TreeSearch.java:53)
        at TreeSearch.solvable(TreeSearch.java:57)
        at TreeSearch.main(TreeSearch.java:72)
        at __SHELL38.run(__SHELL38.java:16)
        at bluej.runtime.ExecServer.suspendExecution(Unknown Source)
</pre>
    </blockquote>
    <p><b>
            <font size="+1"><br />
                Example: Cindy's Puzzle</font>
        </b></p>
    <p>I call the following puzzle "Cindy's puzzle" for historical reasons.
        You have some number<code> n </code>of black marbles and the same number of
        white marbles, and you have a playing board which consists simply of a line
        of<code> 2n+1 </code>spaces to put the marbles in. Start with the black marbles
        all at one end (say, the left), the white marbles all at the other end, and
        a free space in between.</p>
    <table align="center" bgcolor="#dddddd" border="1" cellpadding="6" cellspacing="0">
        <tbody>
            <tr>
                <td width="44"><img src="https://backtracking-html.netlify.app/Black-ball.gif"height="44" width="44" /></td>
                <td width="44"><img src="https://backtracking-html.netlify.app/Black-ball.gif"height="44" width="44" /></td>
                <td width="44">&nbsp;</td>
                <td width="44"><img src="https://backtracking-html.netlify.app/white-ball.gif"height="44" width="44" /></td>
                <td width="44"><img src="https://backtracking-html.netlify.app/white-ball.gif"height="44" width="44" /></td>
            </tr>
        </tbody>
    </table>
    <p>The goal is to reverse the positions of the marbles:</p>
    <table align="center" bgcolor="#dddddd" border="1" cellpadding="6" cellspacing="0">
        <tbody>
            <tr>
                <td width="44"><img src="https://backtracking-html.netlify.app/white-ball.gif"height="44" width="44" /></td>
                <td width="44"><img src="https://backtracking-html.netlify.app/white-ball.gif"height="44" width="44" /></td>
                <td width="44">&nbsp;</td>
                <td width="44"><img src="https://backtracking-html.netlify.app/Black-ball.gif"height="44" width="44" /></td>
                <td width="44"><img src="https://backtracking-html.netlify.app/Black-ball.gif"height="44" width="44" /></td>
            </tr>
        </tbody>
    </table>
    <p>The black marbles can only move to the right, and the white marbles can only
        move to the left (no backing up). At each move, a marble can either:</p>
    <ul>
        <li>Move one space ahead, if that space is clear, or</li>
        <li>Jump ahead over exactly one marble of the opposite color, if the space just
            beyond that marble is clear.</li>
    </ul>
    <p>For example, you could make the following sequence of moves:</p>
    <table align="center" border="0" cellpadding="8" cellspacing="0">
        <tbody>
            <tr>
                <td><b>Starting position:</b></td>
                <td>
                    <table align="center" bgcolor="#dddddd" border="1" cellpadding="6" cellspacing="0">
                        <tbody>
                            <tr>
                                <td width="44"><img src="https://backtracking-html.netlify.app/Black-ball.gif" height="44" width="44" />
                                </td>
                                <td width="44"><img src="https://backtracking-html.netlify.app/Black-ball.gif" height="44" width="44" />
                                </td>
                                <td width="44">&nbsp;</td>
                                <td width="44"><img src="https://backtracking-html.netlify.app/white-ball.gif" height="44" width="44" />
                                </td>
                                <td width="44"><img src="https://backtracking-html.netlify.app/white-ball.gif" height="44" width="44" />
                                </td>
                            </tr>
                        </tbody>
                    </table>
                </td>
            </tr>
            <tr>
                <td><b>Black moves ahead:</b></td>
                <td>
                    <table align="center" bgcolor="#dddddd" border="1" cellpadding="6" cellspacing="0">
                        <tbody>
                            <tr>
                                <td width="44"><img src="https://backtracking-html.netlify.app/Black-ball.gif" height="44" width="44" /></td>
                                <td height="44" width="44">&nbsp;</td>
                                <td width="44"><img src="https://backtracking-html.netlify.app/Black-ball.gif" height="44" width="44" /></td>
                                <td width="44"><img src="https://backtracking-html.netlify.app/white-ball.gif" height="44" width="44" /></td>
                                <td width="44"><img src="https://backtracking-html.netlify.app/white-ball.gif" height="44" width="44" /></td>
                            </tr>
                        </tbody>
                    </table>
                </td>
            </tr>
            <tr>
                <td><b>White jumps:</b></td>
                <td>
                    <table align="center" bgcolor="#dddddd" border="1" cellpadding="6" cellspacing="0">
                        <tbody>
                            <tr>
                                <td width="44"><img src="https://backtracking-html.netlify.app/Black-ball.gif" height="44" width="44" /></td>
                                <td width="44"><img src="https://backtracking-html.netlify.app/white-ball.gif" height="44" width="44" /></td>
                                <td width="44"><img src="https://backtracking-html.netlify.app/Black-ball.gif" height="44" width="44" /></td>
                                <td height="44" width="44">&nbsp;</td>
                                <td width="44"><img src="https://backtracking-html.netlify.app/white-ball.gif" height="44" width="44" /></td>
                            </tr>
                        </tbody>
                    </table>
                </td>
            </tr>
            <tr>
                <td><b>Black moves ahead:</b></td>
                <td>
                    <table align="center" bgcolor="#dddddd" border="1" cellpadding="6" cellspacing="0">
                        <tbody>
                            <tr>
                                <td width="44"><img src="https://backtracking-html.netlify.app/Black-ball.gif" height="44" width="44" /></td>
                                <td width="44"><img src="https://backtracking-html.netlify.app/white-ball.gif" height="44" width="44" /></td>
                                <td height="44" width="44">&nbsp;</td>
                                <td width="44"><img src="https://backtracking-html.netlify.app/Black-ball.gif" height="44" width="44" /></td>
                                <td width="44"><img src="https://backtracking-html.netlify.app/white-ball.gif" height="44" width="44" /></td>
                            </tr>
                        </tbody>
                    </table>
                </td>
            </tr>
            <tr>
                <td><b>Black jumps:</b></td>
                <td>
                    <table align="center" bgcolor="#dddddd" border="1" cellpadding="6" cellspacing="0">
                        <tbody>
                            <tr>
                                <td height="44" width="44">&nbsp;</td>
                                <td width="44"><img src="https://backtracking-html.netlify.app/white-ball.gif" height="44" width="44" /></td>
                                <td width="44"><img src="https://backtracking-html.netlify.app/Black-ball.gif" height="44" width="44" /></td>
                                <td width="44"><img src="https://backtracking-html.netlify.app/Black-ball.gif" height="44" width="44" /></td>
                                <td width="44"><img src="https://backtracking-html.netlify.app/white-ball.gif" height="44" width="44" /></td>
                            </tr>
                        </tbody>
                    </table>
                </td>
            </tr>
            <tr>
                <td><b>White moves ahead:</b></td>
                <td>
                    <table align="center" bgcolor="#dddddd" border="1" cellpadding="6" cellspacing="0">
                        <tbody>
                            <tr>
                                <td height="44" width="44">&nbsp;</td>
                                <td width="44"><img src="https://backtracking-html.netlify.app/white-ball.gif" height="44" width="44" /></td>
                                <td width="44"><img src="https://backtracking-html.netlify.app/Black-ball.gif" height="44" width="44" /></td>
                                <td width="44"><img src="https://backtracking-html.netlify.app/Black-ball.gif" height="44" width="44" /></td>
                                <td width="44"><img src="https://backtracking-html.netlify.app/white-ball.gif" height="44" width="44" /></td>
                            </tr>
                        </tbody>
                    </table>
                </td>
            </tr>
            <tr>
                <td><b>Stuck!</b></td>
                <td>&nbsp; </td>
            </tr>
        </tbody>
    </table>
    <p>Now to the program. The main program will initialize the board, and call a
        recursive backtracking routine to attempt to solve the puzzle. The backtracking
        routine will either succeed and print out a winning path, or it will fail, and
        the main program will have to print out the bad news.</p>
    <p>The backtracking method is named <code>solvable</code> and returns a <code>boolean</code>.
        In <code>solvable</code> we shall need to check whether we are at a leaf, which
        in this case means a position from which no further moves are possible. This
        isn't so easy.</p>
    <p>Each possible move will result in a new board position, and these new board
        positions are the children of the current board position. Hence to find the
        children of a node (that is, of a board position), we need only find the possible
        moves from that node. Remember that it is also highly desirable to find an ordering
        on these possible moves.</p>
    <p>Here it is time to stop and take thought. To make progress, we must analyze
        the game to some extent. Probably a number of approaches would work, and what
        follows is based on the way I worked it out. If you were to program this puzzle,
        you might find a different but equally valid approach.</p>
    <p>First, notice that if a marble has a move, that move is unique: if it can move
        ahead one square, then it cannot jump. If it can jump, it cannot move ahead
        one square. This suggests that, to find the possible moves, we might assign
        numbers to the marbles, and check each marble in turn. When we have looked at
        all the marbles, we have looked at all the possible moves. This would require
        having a table to keep track of where each marble is, or else somehow "marking"
        each marble with its number and searching the board each time to find the marble
        we want. Neither alternative is very attractive.</p>
    <p>Next, notice that for a given board position, each marble occupies a unique
        space. Hence, instead of talking about moving a particular marble, we can talk
        about moving the marble in a particular space. If a move is possible from a
        given space, then that must be the only move possible from that space, because
        if the marble in that space has a move, it is unique. There is a slight complication
        because not every space contains a marble, but at least the spaces (unlike the
        marbles) stay in one place. </p>
    <p>Now we have a simpler ordering of moves to use in our program. Just check,
        in order, the <code>2n+1 </code>spaces of the board. For each space, either
        zero or one moves is possible. With this understanding, we can write a boolean
        method<code> canMove(int[]&nbsp;board,&nbsp;int&nbsp;position) </code>which
        determines whether a move is possible from the given position:</p>
    <ul>
        <li>If the position is empty, no move is possible;</li>
        <li>If the position contains a black marble, the method checks for a move or
            jump to the right;</li>
        <li>If the position contains a white marble, the method checks for a move or
            jump to the left.</li>
    </ul>
    <p>We write another method <code>int[]&nbsp;makeMove(int[]&nbsp;oldBoard,&nbsp;int&nbsp;position) 
  </code>that will take a board and a position, make a move from that position,
        and return as its value a <i>new</i> board. (We could write this somewhat more
        efficiently by changing the old board, rather than creating a new one, but here
        we are more concerned with simplicity.) In technical jargon, <code>makeMove</code>
        is "applicative" rather than "mutative."</p>
    <p>With these methods, our central backtracking method can be written as follows:</p>
    <blockquote>
        <pre>boolean solvable(int[] board) {
    if (puzzleSolved(board)) {
        return true;
    }
    for (int position = 0; position &lt; BOARD_SIZE; position++) {
        if (canMove(board, position)) {
            int[] newBoard = makeMove(board, position);
            if (solvable(newBoard)) {
                printBoard(newBoard);
                return true;
            }
        }
    }
    return false;
}</pre>
    </blockquote>
    <p>Along with <code>canMove</code> and <code>makeMove</code>, we are using methods
        <code>puzzleSolved</code> and <code>printBoard</code> with meanings that should
        be obvious.
    </p>
    <p>Here is some output from the program:</p>
    <pre><i><font color="#0000ff">16.</font>  </i><b><font color="#000000">WHITE  WHITE  WHITE  _____  BLACK  BLACK  BLACK</font></b>
<i><font color="#0000ff">15.</font></i>  <b><font color="#000000">WHITE  WHITE  WHITE  BLACK  _____  BLACK  BLACK</font></b>
<i><font color="#0000ff">14.</font></i>  <b><font color="#000000">WHITE  WHITE  _____  BLACK  WHITE  BLACK  BLACK</font></b>
<i><font color="#0000ff">13.</font></i>  <b><font color="#000000">WHITE  _____  WHITE  BLACK  WHITE  BLACK  BLACK</font></b>
<i><font color="#0000ff">12.</font></i>  <b><font color="#000000">WHITE  BLACK  WHITE  _____  WHITE  BLACK  BLACK</font></b>
<i><font color="#0000ff">11. </font></i> <b><font color="#000000">WHITE  BLACK  WHITE  BLACK  WHITE  _____  BLACK</font></b>
<i><font color="#0000ff">10.</font></i> <b><font color="#000000">WHITE  BLACK  WHITE  BLACK  WHITE  BLACK  _____</font></b>
<i><font color="#0000ff"> 9. </font></i> <b><font color="#000000">WHITE  BLACK  WHITE  BLACK  _____  BLACK  WHITE</font></b>
<i><font color="#0000ff"> 8. </font></i> <b><font color="#000000">WHITE  BLACK  _____  BLACK  WHITE  BLACK  WHITE</font></b>
<i><font color="#0000ff"> 7. </font></i> <b><font color="#000000">_____  BLACK  WHITE  BLACK  WHITE  BLACK  WHITE</font></b>
<i><font color="#0000ff"> 6. </font></i> <b><font color="#000000">BLACK  _____  WHITE  BLACK  WHITE  BLACK  WHITE</font></b>
<i><font color="#0000ff"> 5. </font></i> <b><font color="#000000">BLACK  BLACK  WHITE  _____  WHITE  BLACK  WHITE</font></b>
<i><font color="#0000ff"> 4. </font></i> <b><font color="#000000">BLACK  BLACK  WHITE  BLACK  WHITE  _____  WHITE</font></b>
<i><font color="#0000ff"> 3. </font></i> <b><font color="#000000">BLACK  BLACK  WHITE  BLACK  _____  WHITE  WHITE</font></b>
<i><font color="#0000ff"> 2.</font></i>  <b><font color="#000000">BLACK  BLACK  _____  BLACK  WHITE  WHITE  WHITE</font></b>
<i><font color="#0000ff"> 1. </font></i> <b><font color="#000000">BLACK  BLACK  BLACK  _____  WHITE  WHITE  WHITE</font></b>
</pre>
    <p>Notice that the solution is given in reverse order: BLACK starts out on the
        left and WHITE on the right, as in the last line. I've added line numbers to
        the actual output in order to emphasize this point. Backtracking always produces
        its results (sequence of choices) in reverse order; it is up to you, the programmer,
        to reverse the results again to get them in the correct order.</p>
       
</body>

</html>
