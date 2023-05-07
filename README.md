Download Link: https://assignmentchef.com/product/solved-options-b1-and-b2
<br>
You are going to be changing the general tree class template, <strong>FHtree</strong>, of the lectures by adding a single member to it:

<pre class="ql-syntax"><span style="color: #ff0000;">   <span class="hljs-keyword">boolean</span> deleted; <span class="hljs-comment">// true if the node has been removed from the tree</span></span></pre>

The idea is that when a client asks this class to delete a node, all we will do is set this boolean to <strong>true</strong> and return without further ado.

You will have to rename the classes from <strong>FHtreeNode</strong> and <strong>FHtree</strong> (from the lectures) to  <strong>FHsdTreeNode</strong> and <strong>FHsdTree</strong>, respectively, to distinguish them from the normal-deletion implementations in those lectures.

That’s it in a nutshell. The purpose of this new, <strong>deleted</strong>, member is to avoid ever physically removing a node when the client wants to delete it. Instead we just set <strong>deleted = true</strong> and it will be considered “gone”.

Of course, this is like passing a law. Unless everyone obeys the law, it won’t have any effect. So we have to modify most of the methods to take advantage of it.

Some might be easier, some not. For example,<strong> remove()</strong> may be simpler and shorter than before, since, once it finds the node, all it has to do is set <strong>deleted = true</strong> — no actual unlinking of the nodes, destruction of objects, etc.   Other methods, like <strong>find()</strong>, will be more subtle. <em>Finding</em> a node means <em>more</em> than matching the data (<strong> if (x.equals(root.data)) …</strong> ). Even though we might find the data, <strong>x</strong>, in the tree, this does not mean <strong>x</strong> is really there. It may have been <strong>deleted</strong> at some point in the past. Remember: <em>now</em>, deleting it leaves it physically in the tree with the <strong>deleted</strong> flag set. You have to add a second test,<strong> if (deleted) …</strong>, before you know whether or not you have really found <strong>x</strong>. Same thing with <strong>display()</strong>, <strong>size()</strong>, etc. They will only report on the virtual tree, not the physical one that is stored in memory. If a node is deleted, it may not be counted or displayed by the various methods.

On the other hand, there will be some new methods that don’t have general tree counterparts.  <strong>sizePhysical()</strong> will report on the size of the tree, counting both deleted and non-deleted nodes (so it will basically be exactly our old <strong>size()</strong> method. <strong> displayPhysical()</strong> will likewise show everything, including deleted nodes (it will not be required to show the deleted status for this assignment, although that’s a nice touch).  <strong>displayPhysical()</strong>, also, can be engineered to reuse the code of the old <strong>display()</strong> method since that method, by virtue of its ignorance of the newly added <strong>deleted</strong> flag, already shows all the nodes.

In soft deletion, trees don’t normally ever shrink; they keep getting larger. Even though the client may be adding and removing the same number of objects, the <strong>add()</strong>s will <em>always</em> add nodes, but the<strong> remove()</strong>s will <em>never</em> take nodes away, just mark them as deleted. This is the meaning and expected behavior of <em>soft deletion</em>. It is used when normal deletion (which we can now call <em>hard or physical deletion</em> to emphasize the difference) has some disruptive effect on the tree and we want to avoid such physical disruption for some reason.

[When we get to <strong>binary trees</strong> and<strong> probing hash tables</strong> in the next course, we will be able to re-use the deleted nodes when a client wants to add something that is physically present but marked as deleted. However, for general trees, this refinement is both trickier and less useful, so there is no need for us to burden ourselves with that technique.]

Finally, we will need a <strong>collectGarbage()</strong> method that goes through the entire tree and physically removes all deleted nodes. This is the only method that will shrink the tree size and bring its physical and virtual contents, at least temporarily, into alignment.

For your convenience, the complete listing of the hard-deletion-based general tree presented in the lectures can he downloaded here

zipped FHgTree files (Links to an external site.)

Links to an external site.

Download the .zip file, unziop it and drag a copy of <strong>FHtree.java</strong>, <strong>FHtreeNode.java</strong> and <strong>Traverser.java</strong> into your project’s <strong>src</strong> folder. Then, incorporate these classes into your project (in Eclipse, use <strong>File → Refresh</strong>). Also, copy and past the contents of the client, <strong>Foothill.java</strong> into <em>your</em> main <strong>.java</strong> file. If your main class is not named <strong>Foothill</strong>, make adjustments, accordingly. If you compile and run the project after doing this, you should get the original output as shown in the modules.

Understand the Client

The client <strong>main()</strong> that is provided in the lesson (and in your zipped download file, above) will eventually need to be modified in two ways for this assignment:

<ol>

 <li>Any references to <strong>FHtreeNode</strong> and <strong>FHtree</strong> in the downloaded client must be changed to refer to the new classes we are designing,  <strong>FHsdTreeNode</strong> and <strong>FHsdTree</strong>.</li>

 <li>The client will contain some extra tests in addition to the ones that were there. For example, we’ll need to write and test methods like<strong> displayPhysical()</strong>, <strong>collectGarbage()</strong>.</li>

</ol>

<u>Implementation Details</u>

After you succeed in compiling and running the lecture code as described above, rename <strong>FHtree.java</strong> to  <strong>FHsdTree.java</strong>, and rename and <strong>FHtreeNode.java</strong> to  <strong>FHsdTreeNode.java</strong>  Change the names of the classes that these files contain to <strong>FHsdTreeNode</strong> and <strong>FHsdTree</strong>.  [<strong>Note</strong>: Eclipse, will do all this for you <em>automatically</em> once you <strong>Refactor → Rename</strong> each file, but other IDEs might require that you adjust the contents of all the files, manually, to accommodate the new file names.] Do whatever it takes to both these <strong>.java</strong> files and your main  <strong>.java</strong> file so that everything compiles with these new names (but don’t attempt any new implementation yet). Run again to make sure you get the same output as before. You are now ready to begin your assignment.

Class FHsdTreeNodeData Members:

<ul>

 <li>All previous members remain plus the one new private (or protected if you prefer) data member <strong>boolean deleted</strong>.</li>

</ul>

Methods To Modify:

All parameter-taking constructors should be modified to take a new parameter for the <strong>deleted</strong> member, and the default constructor should make sure the value for the <strong>deleted</strong> member is initialized to  <strong>false</strong>.

Class FHsdTreeOverriding Methods:

<ul>

 <li><strong>addChild()</strong> – not much difference from old <strong>addChild()</strong>. In particular, we do not see if the data is already in the tree as <strong>deleted</strong> and get clever. We always add the node, exactly as if this were an ordinary tree.  However, optionally, you can check the first parameter (<strong>treeNode</strong>, <strong>root</strong>, or whatever you call it) to see if it is deleted, and reject the <em>add</em> if it is. There are some unusual scenarios in which the client might inadvisably attempt to make a useless addition under/to a deleted node.</li>

 <li><strong>find()</strong> – Two versions, just like the old implementation: one non-recursive takes only the data, <strong>x</strong>, and the other which takes two more parameters for recursion. The non-recursive calls the recursive. The recursive method should check the <strong>deleted</strong> flag of any data match (a “found<strong>” x</strong>) and, if <strong>true</strong>, should return <em>as if the data were not in the tree.</em> If the deleted flag is <strong>false</strong>, then you would return the node as before, a “good find”.  Note: the sub-tree of a deleted node is considered entirely deleted, even if individual sub-tree nodes might be still have <strong>deleted = false</strong>.</li>

 <li><strong>remove()</strong> – Two versions, just like the previous implementation of a general tree, however, the <strong>deleted</strong> flag of the appropriate node is set to true.  <em>The node is not physically removed from the tree. </em> Note the meaning: this has the same meaning as the old <strong>remove()</strong>. If a node is marked as deleted, then the entire child sub-tree is considered <em>gone</em>. Those nodes, while not marked themselves (big error to waste time marking them), are no longer in the tree.  <strong>Caution</strong>: While you might think it best to mark the <strong>firstChild</strong> of the deleted node to <strong>null</strong>, thereby allowing the Java garbage collector to get rid of that subtree for you, <em>do not do this</em>. Leave the <strong>firstChild</strong>, and all the children still in the physical tree. In CS 1C, we will see how we can reuse deleted nodes left in the tree to save work. This forces us to write our other methods (like <strong>display()</strong> and <strong>find()</strong>) to obey the <strong>deleted</strong> flag and consciously skip processing children of a deleted node.</li>

 <li><strong>Others</strong> — any other method which require overriding must be overridden. You have to decide. Using common sense on the meaning and interpretation of <strong>deleted</strong> is part of your assignment. There are several methods that require overriding.  <strong>Hint</strong>:  <strong>size()</strong> will not use <strong>mSize</strong>, since that member only reflects physical size. Instead, <strong>size()</strong> has to <em>compute</em> the size, manually (read “recursively”), so it is harder than the old <strong>size()</strong>. <strong> traverse()</strong> is another that needs tweaking. Check for more.</li>

</ul>

New Methods:

“Physical” versions of some methods are needed so we can (for debugging and maintenance purposes) see the actual tree on the console, replete with both deleted and undeleted nodes. Some of these methods work almost exactly as the prior implementation of methods in the non-soft-deletion trees.

<ul>

 <li><strong>sizePhysical()</strong> – returns the actual, physical size. This is easy: just like our old<strong> size()</strong> from the lectures. It just has a new name in this regime since the old name,<strong> size()</strong>, is now used to return the virtual size of the tree (a count of non-deleted nodes — and remember, all children/sub-trees of deleted nodes are considered deleted, <em>even if their </em><strong><em>deleted</em></strong><em> flag is still set to </em><strong><em>false</em></strong>).</li>

 <li><strong>displayPhysical()</strong> – like the old <strong>display()</strong> from the lectures. Ignores the <strong>deleted</strong> flag. Optionally, place a ” (D)” after any node that has<strong> deleted == true</strong>.  <strong>Note: </strong> you don’t have to add the ” (D)” if the node is not <em>marked</em> as deleted, even though it might <em>actually</em> be deleted by virtue of its being in a subtree of a deleted node.</li>

 <li><strong>collectGarbage()</strong> – physically removes all nodes that are marked as <strong>deleted</strong>. After this method is called, the physical and virtual size of the tree would be the same. A physical and virtual <strong>display()</strong> would also be the same after a call to <strong>collectGarbage()</strong> (at least temporarily, until more items are<strong> remove()</strong>ed from the tree). This method does exactly what it says: it takes out the garbage.<strong> Hint:</strong> make use of <strong>removeNode()</strong>, which can be preserved from the original general tree class.</li>

</ul>

<u>Suggestion about Order of Implementation</u>

<ol>

 <li>Add the <strong>deleted</strong> member to the <strong>FHsdTreeNode</strong> class as private or protected.</li>

 <li>Update the signatures of the constructors <strong>FHsdTreeNode()</strong>, to accommodate the extra parameter (wherever you want to put it).</li>

 <li>One at-a-time, begin updating the <strong>FHsdTree</strong> methods as demanded by the new <strong>FHsdTreeNode</strong> member, <strong>deleted</strong>. Suggestion: Start with <strong>addChild()</strong>, then <strong>find()</strong>, then<strong> remove()</strong>. Test as you go. Once these three are working, you’ll implement all the rest.</li>

</ol>

Client

Here is a client you can use to test your code. I am not going to tell you what this client should produce. You should be able to predict that on your own and make a judgment about whether your code is working. At this point in the second programming course, I expect you to be able to make these kinds of determinations. This is the minimal client for testing, but you can add to it, optionally, if you wish.

<pre class="ql-syntax">import java.text.*;import java.util.*;<span class="hljs-comment">//------------------------------------------------------</span><span class="hljs-keyword">public</span> <span class="hljs-keyword">class</span> <span class="hljs-title">Foothill</span>{    <span class="hljs-comment">// -------  main --------------</span>   <span class="hljs-function"><span class="hljs-keyword">public</span> <span class="hljs-keyword">static</span> <span class="hljs-keyword">void</span> <span class="hljs-title">main</span>(<span class="hljs-params">String[] args</span>) throws Exception   </span>{      FHsdTree&lt;String&gt; sceneTree = <span class="hljs-keyword">new</span> FHsdTree&lt;String&gt;();      FHsdTreeNode&lt;String&gt; tn;            System.<span class="hljs-keyword">out</span>.println(<span class="hljs-string">"Starting tree empty? "</span> + sceneTree.empty() + <span class="hljs-string">"n"</span>);      <span class="hljs-comment">// create a scene in a room</span>      tn = sceneTree.addChild(<span class="hljs-literal">null</span>, <span class="hljs-string">"room"</span>);      <span class="hljs-comment">// add three objects to the scene tree</span>      sceneTree.addChild(tn, <span class="hljs-string">"Lily the canine"</span>);      sceneTree.addChild(tn, <span class="hljs-string">"Miguel the human"</span>);      sceneTree.addChild(tn, <span class="hljs-string">"table"</span>);      <span class="hljs-comment">// add some parts to Miguel</span>      tn = sceneTree.find(<span class="hljs-string">"Miguel the human"</span>);      <span class="hljs-comment">// Miguel's left arm</span>      tn = sceneTree.addChild(tn, <span class="hljs-string">"torso"</span>);      tn = sceneTree.addChild(tn, <span class="hljs-string">"left arm"</span>);      tn =  sceneTree.addChild(tn, <span class="hljs-string">"left hand"</span>);      sceneTree.addChild(tn, <span class="hljs-string">"thumb"</span>);      sceneTree.addChild(tn, <span class="hljs-string">"index finger"</span>);      sceneTree.addChild(tn, <span class="hljs-string">"middle finger"</span>);      sceneTree.addChild(tn, <span class="hljs-string">"ring finger"</span>);      sceneTree.addChild(tn, <span class="hljs-string">"pinky"</span>);      <span class="hljs-comment">// Miguel's right arm</span>      tn = sceneTree.find(<span class="hljs-string">"Miguel the human"</span>);      tn = sceneTree.find(tn, <span class="hljs-string">"torso"</span>, <span class="hljs-number">0</span>);      tn = sceneTree.addChild(tn, <span class="hljs-string">"right arm"</span>);      tn =  sceneTree.addChild(tn, <span class="hljs-string">"right hand"</span>);      sceneTree.addChild(tn, <span class="hljs-string">"thumb"</span>);      sceneTree.addChild(tn, <span class="hljs-string">"index finger"</span>);      sceneTree.addChild(tn, <span class="hljs-string">"middle finger"</span>);      sceneTree.addChild(tn, <span class="hljs-string">"ring finger"</span>);      sceneTree.addChild(tn, <span class="hljs-string">"pinky"</span>);      <span class="hljs-comment">// add some parts to Lily</span>      tn = sceneTree.find(<span class="hljs-string">"Lily the canine"</span>);      tn = sceneTree.addChild(tn, <span class="hljs-string">"torso"</span>);      sceneTree.addChild(tn, <span class="hljs-string">"right front paw"</span>);      sceneTree.addChild(tn, <span class="hljs-string">"left front paw"</span>);      sceneTree.addChild(tn, <span class="hljs-string">"right rear paw"</span>);      sceneTree.addChild(tn, <span class="hljs-string">"left rear paw"</span>);      sceneTree.addChild(tn, <span class="hljs-string">"spare mutant paw"</span>);      sceneTree.addChild(tn, <span class="hljs-string">"wagging tail"</span>);      <span class="hljs-comment">// add some parts to table</span>      tn = sceneTree.find(<span class="hljs-string">"table"</span>);      sceneTree.addChild(tn, <span class="hljs-string">"north east leg"</span>);      sceneTree.addChild(tn, <span class="hljs-string">"north west leg"</span>);      sceneTree.addChild(tn, <span class="hljs-string">"south east leg"</span>);      sceneTree.addChild(tn, <span class="hljs-string">"south west leg"</span>);      System.<span class="hljs-keyword">out</span>.println(<span class="hljs-string">"n------------ Loaded Tree ----------------- n"</span>);      sceneTree.display();            sceneTree.<span class="hljs-keyword">remove</span>(<span class="hljs-string">"spare mutant paw"</span>);      sceneTree.<span class="hljs-keyword">remove</span>(<span class="hljs-string">"Miguel the human"</span>);      sceneTree.<span class="hljs-keyword">remove</span>(<span class="hljs-string">"an imagined higgs boson"</span>);            System.<span class="hljs-keyword">out</span>.println(<span class="hljs-string">"n------------ Virtual (soft) Tree --------------- n"</span>);      sceneTree.display();      System.<span class="hljs-keyword">out</span>.println(<span class="hljs-string">"n----------- Physical (hard) Display ------------- n"</span>);      sceneTree.displayPhysical();            System.<span class="hljs-keyword">out</span>.println(<span class="hljs-string">"------- Testing Sizes (compare with above) -------- n"</span>);      System.<span class="hljs-keyword">out</span>.println(<span class="hljs-string">"virtual (soft) size: "</span> + sceneTree.size()  );      System.<span class="hljs-keyword">out</span>.println(<span class="hljs-string">"physiical (hard) size: "</span> + sceneTree.sizePhysical()  );      System.<span class="hljs-keyword">out</span>.println(<span class="hljs-string">"------------ Collecting Garbage ---------------- n"</span>);      System.<span class="hljs-keyword">out</span>.println(<span class="hljs-string">"found soft-deleted nodes? "</span>             + sceneTree.collectGarbage()  );      System.<span class="hljs-keyword">out</span>.println(<span class="hljs-string">"immediate collect again? "</span>             + sceneTree.collectGarbage()  );      System.<span class="hljs-keyword">out</span>.println(<span class="hljs-string">"--------- Hard Display after garb col ------------ n"</span>);      sceneTree.displayPhysical();      System.<span class="hljs-keyword">out</span>.println(<span class="hljs-string">"Semi-deleted tree empty? "</span> + sceneTree.empty() + <span class="hljs-string">"n"</span>);      sceneTree.<span class="hljs-keyword">remove</span>(<span class="hljs-string">"room"</span>);      System.<span class="hljs-keyword">out</span>.println(<span class="hljs-string">"Completely-deleted tree empty? "</span> + sceneTree.empty()          + <span class="hljs-string">"n"</span>);            sceneTree.display();   }}</pre>

<strong>OPTION B-1 (Intermediate) </strong>

This is instead of, not in addition to, <strong>Option A</strong>.

Implement the above soft deletion using <em>derived</em> class generics:

<pre class="ql-syntax">   public <span class="hljs-class"><span class="hljs-keyword">class</span> <span class="hljs-title">FHsdTreeNode</span>&lt;E&gt; <span class="hljs-title">extends</span> <span class="hljs-title">FHtreeNode</span>&lt;E&gt;</span>   public <span class="hljs-class"><span class="hljs-keyword">class</span> <span class="hljs-title">FHsdTree</span>&lt;E&gt; <span class="hljs-title">extends</span> <span class="hljs-title">FHtree</span>&lt;E&gt; <span class="hljs-title">implements</span> <span class="hljs-title">Cloneable</span></span></pre>

In this solution, you may not touch a single character or the base classes – download them by using the same link given in <strong>Option A</strong>, but don’t modify them after they are copied into your project directory. Thus your project will have the old <strong>FHtree.java</strong> and<strong> FHtreeNode.java</strong> files, as well as new file <strong>FHsdTreeNode.java</strong> and <strong>FHsdTree.java</strong> files<strong> t</strong>hat you will write. These two files will have the derived classes <strong>FHsdTreeNode</strong> and <strong>FHsdTree</strong> defined. This is a realistic approach that prepares you for a situation in which you do not have access to the original source. The only members that the derived class may have are as follows:

Class FHsdTreeNode:

<ol>

 <li><strong>boolean deleted</strong> (private or protected).</li>

 <li></li>

 <li><strong>at least one constructor</strong> that takes all parameters, chains to base class, and sets the deleted flag manually, by parameter. Default constructor overload, okay. (You won’t need the protected constructor because of the addition of the mutator in the next step, but it’s okay if you decide to add it in.)</li>

 <li></li>

 <li><strong>accessors</strong> and <strong>mutators</strong> for members like <strong>firstChild</strong>, <strong>prev</strong>, <strong>myRoot</strong>, etc.</li>

</ol>

Class FHsdTree

<ol>

 <li>No new data members or statics.</li>

 <li></li>

 <li>Overrides of <strong>find()</strong>,<strong> remove()</strong>, <strong>addChild()</strong>,<strong> size()</strong>, etc. (Whatever was needed in <strong>Option A</strong> is needed here.)</li>

 <li></li>

 <li>Adds<strong> displayPhysical()</strong>, <strong>sizePhysical()</strong> and <strong>collectGarbage(</strong>)</li>

</ol>

These methods work exactly like their single-class counterparts of <strong>Option A</strong>, but are now in the derived class.

Client

The client will not change from <strong>Option A</strong>.

Use the organization and methods in <strong>FHtree.java</strong> and <strong>FHtreeNode.java</strong> as a guide for declaring your two new classes, but remember, these new classes extend the old ones.

<strong>Warning:</strong> This is harder than it looks. Your new classes are not “friends” with the given classes, and they might not even be in the same package. The data members of the <strong>FHtreeNode</strong> class will not be visible to the <strong>FHsdTree</strong> class, and you can’t provide accessors in <strong>FHtreeNode.java</strong> since, in<strong> Option B1</strong>, you don’t have permission to touch the base class source. This is why I direct you to provide accessors in the <em>derived</em> <strong>FHsdTreeNode</strong> class for these data members in the base class. You might, incidentally, seem to be able to do without the accessors, but if so, that would only be because your <strong>FHsdTree</strong> class and <strong>FHtreeNode</strong> class happen to be in the same package. In general, they won’t be.  Therefore, you have to constantly find ways in the <strong>FHsdTree</strong> methods to “get at” either the derived members or the new <strong>deleted</strong> member, as needed.

OPTION B-2 (Intermediate)

To either option above, demonstrate two more <strong>main()</strong>s, in addition to the first, that create a tree of different types:

<ol>

 <li>A simple numeric type</li>

 <li></li>

 <li>Some user-defined <strong>Object</strong> type (like <strong>iTunes</strong>, <strong>Employee</strong> or whatever you want to provide).</li>

</ol>