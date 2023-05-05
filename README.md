Download Link: https://assignmentchef.com/product/solved-cse522-assignment-3-thread-safe-trees-and-readers-writers-with-writers-priority
<br>
<strong><em>Part 1: Thread Safe Trees </em></strong>

Refer to file ThreadSafeTree.java whose main method creates and starts five concurrent threads, each of which inserts five random integers into a tree, and finally prints out all values in the tree.   The code as given invokes the standard Tree.insert method, which is <em>not thread-safe</em>, meaning that it is not suited for concurrent insertion of values into the tree, as values will be dropped from the tree due to the lack of any synchronization – as can be observed from the object diagram.

A preliminary solution to this problem is to declare insert as a synchronized method.  Doing so will solve the problem of dropped values, but this solution is not desirable because it sacrifices concurrency – essentially, all values are now inserted sequentially into the tree.   You might try this out as a preliminary step.

Your task in Part 1 is to write a subclass SafeTree of class Tree with a <em>thread-safe</em> definition of insert(n), i.e., it preserves the basic logic of Tree.insert, but permits concurrent insertion of values into the tree, with no dropped values.  Concurrent insertion into disjoint subtrees as well as concurrent insertion at different nodes along any path from the root to a leaf should be supported.  To meet these objectives:

<ol>

 <li>Do not declare insert(n) as a synchronized method.</li>

 <li>Define two synchronized methods in class SafeTree, called lock() and unlock(), using which insert(n) can ensure that one thread at a time is accessing any given</li>

</ol>

SafeTree object, but other SafeTree objects can be accessed concurrently by other threads.

<ol start="3">

 <li>Call the lock() and unlock() methods from insert(n) in such a way that any SafeTree object is locked for as short a duration as possible.</li>

 <li>Define lock() and unlock() using Java’s wait-notify Their definitions are similar to other wait-notify examples discussed in the lectures.</li>

</ol>

Run ThreadSafeTree.java to completion after replacing the instruction ‘new Tree(5000)’ by ‘new SafeTree(5000)’ in ThreadSafeTree.main.   Proceed as follows:

<ol>

 <li>Check the JIVE object diagram to make sure that it does not have any dropped values, and check the console output to make sure that the printed values are in ascending order.</li>

 <li>Bring up the JIVE Search window, choose the <em>Object Created</em> option, enter SafeTree for the class name, and press <em>Search</em>. There should be 31 entries in the Search Results window for the given test case.</li>

 <li>Step through the search results one by one, observing the object diagram (using the ‘<em>Objects’</em> option) at each step, until you locate at an object diagram showing <em>maximal concurrency</em>, i.e., where there is a maximum number of active threads in disjoint subtrees and also a maximum number of active threads at different nodes along a path in the tree. There could be more than one such diagram; choose any object diagram with <em>maximal concurrency</em>.</li>

 <li>Save the chosen object diagram from step 3 in a file called png.</li>

</ol>

<strong><em>Part 2: Readers-Writers with Writers Priority </em></strong>

The Readers-Writers problem, discussed in Lecture 14, is a classic example in the study of concurrency control.   There are two types of concurrent threads, reader threads (readers) and writer threads (writers), and they concurrently access a database.  Readers execute the database ‘read’ operation while writers execute the database ‘write’ operation.   The basic requirement of concurrency control is that a ‘read’ operation may be executed concurrently by two or more readers, but a ‘write’ operation should not be executed concurrently with any ‘read’ or any ‘write’ operation.




An important variant of the basic problem is the Readers-Writers problem with Writers Priority.   Here, when a writer tries to access the database and is made to wait because either a read or write is in progress, all subsequent read requests are delayed until this writer gets to access the database.  In other words, a waiting writer takes precedence over every waiting reader regardless of the order of their arrival.  Note: Active (or, running) readers are not pre-empted but are allowed to complete their read operations before the waiting writer begins its operation.




Posted on Piazza is a file ReadersWriters.java containing a complete implementation of the Readers-Writers problem with Writers Priority.  This program is written with wait-notify constructs.   Your task in Part 2 is to translate all synchronized methods and wait-notify constructs in terms Java Semaphores using the methodology outlined in Lecture 14 slide 31.  Name your semaphores as follows:




<ol>

 <li>Use one semaphore (named s1) for translating synchronized methods.</li>

</ol>




<ol start="2">

 <li>Use one semaphore (named s2_r) for waiting readers and another semaphore (named s2_w) for waiting writers.</li>

</ol>




<ol start="3">

 <li>Implement notifyAll by performing release(s) on the appropriate semaphores, as explained in Lecture 14 slide 31.</li>

</ol>




Name your translated program as ReadersWritersSemaphore.java.




Install in Eclipse the <em>State Diagram and Property Checker plugin</em> posted on Piazza following the instructions posted at Resources → Software Tools → FSM_Installation_Usage.pdf, and proceed as follows:




<ol>

 <li>Run the program to completion and check that the data field in object Database:1 has the value 55555 for the given test case.</li>

</ol>




<ol start="2">

 <li>Save the<em> Execution Trace</em> in a file called csv and load this file into the <em>Property Checker</em> – see the abovementioned file on installation instructions and usage.</li>

</ol>




<ol start="3">

 <li>From the dropdown menu, choose the fields</li>

</ol>




Database:1.r, Database:1.w,  Database:1.ww

Enlarge the Canvas Dimension (using the text boxes at bottom of diagram), and draw.




<ol start="4">

 <li>In the <em>Abbreviations</em> text box enter (but avoid cutting and pasting from PDF file):</li>

</ol>




Database:1.r = r, Database:1.w = w, Database:1.ww = ww

<ol start="5">

 <li>In the <em>Properties</em> textbox, enter (but avoid cutting and pasting from PDF file):</li>

</ol>




G [ (w == 1 -&gt; r == 0) &amp;&amp;

(r &gt; 0 -&gt; w == 0)  &amp;&amp;

(w == 0 || w == 1) &amp;&amp;

(r &gt; 0 &amp;&amp; ww &gt; 0 -&gt; r’ &lt;= r)

]




<ul>

 <li>The first three conjuncts express the basic readers-writers concurrency control policy, and the last conjunct expresses the writers-priority condition.</li>

 <li>The variable r’ refers to the value of r in the next state, and the condition states that the number of running readers must monotonically decrease when there are waiting writers.</li>

 <li>The outermost G specifies that this property must hold globally, i.e., in all states.</li>

</ul>




<ol start="6">

 <li>Press Validate and look for the message <em>“All properties satisfied.”</em> below the Properties textbox. If this message does not appear, look for states highlighted in red in the state diagram, as they will help determine the cause of failure.    (The topmost state is always redhighlighted when a G property fails.)   Use this information to correct your program.</li>

</ol>




<ol start="7">

 <li>When all properties are satisfied, save the diagram in a file called svg. This can be done by right-clicking on the diagram and choosing Save picture as…</li>

</ol>




<ol start="8">

 <li>Reset previous choices and, again, from the dropdown menu, choose the fields</li>

</ol>




Database:1.r, Database:1.w,  Database:1.data




<ol start="9">

 <li>Draw the State Diagram and save it to a file called png. This diagram helps visually check the Writers priority condition – the diagram should slope to the right and end with a long tail of read operations.</li>

</ol>