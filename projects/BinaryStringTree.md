---
layout: project
type: project
image: img/BinaryStringTree/binary-300x274-square.png
title: "BinaryStringTree"
date: 2024-04-11
published: true
labels:
  - Java
summary: "A program created in ICS 211 that finds the top 10 most used word in literature"
---
This project has taught me how trees can be used to efficiently store and manipulate data which plays a big role in solving real world problems that involve text analysis. the text is read and each word is stored in the tree in it's lexicographical order which allows for binary search with logarithmic time. A tweak in the program was made prior to running to achieve a balanced tree and this really important because the height of the tree is calculated to be 24 which is significantly higher than a perfectly balanced tree which equals to around 11. 

Here is the source code for the program:

```
package edu.ics211.h11;

import java.io.File;
import java.io.FileNotFoundException;
import java.util.Arrays;
import java.util.Comparator;
import java.util.Scanner;
import java.util.Set;

/**
 * @author Phuong Pham
 *
 */

public class BinaryStringTree {

	private static class BinaryNode {
		private String key;
		private long value;
		private BinaryNode left;
		private BinaryNode right;


		/** 
		 * constructor to build a node with no subtrees
		 * @param the value to be stored by this node
		 */
		private BinaryNode(String key, long value) {
			this.key = key;
			this.value = value;
			left = null;
			right = null;
		}


		/** 
		 * constructor to build a node with a specified (perhaps null) subtrees
		 * @param the value to be stored by this node
		 * @param the left subtree for this node
		 * @param the right subtree for this node
		 */
		private BinaryNode(String key, long value, BinaryNode l, BinaryNode r) {
			this.key = key;
			this.value = value;
			left = l;
			right = r;
		}
	}


	// Class variable
	private BinaryNode root;


	// Empty constructor
	public BinaryStringTree() { }


	// Constructor that reads file
	public BinaryStringTree(String fileName) throws FileNotFoundException {

		File file = new File(fileName); // Create a File object with the filename
		Scanner reader = new Scanner(file);  // Scanner to read our file

		while(reader.hasNext()) {
			StringBuilder sb = new StringBuilder();
			String word = reader.next();  // Get the next word from the scanner
			// Check each character in the word, only append alphabetic characters

			for ( int i = 0; i < word.length(); i++) {
				char c = word.charAt(i);  // Pull each character individually
				// If the character is alphabetic, append to the StringBuilder
				if(Character.isJavaIdentifierStart(c)) {
					sb.append(c);
				}
			}

			// If the word we built is not empty, add it to the tree
			if( sb.length() > 0) {
				add(sb.toString());
			}

		}
	}


	/**
	 * 
	 * @param key
	 * @return the number of occurrences of the given String, or
	 *         0 if the String is not found
	 */
	public long occurrences(String key) {
		//TODO: call a private recursive helper method that you design
		return occurrences(key, root);
	}


	// occurences() helper method, using recursion
	private long occurrences(String key, BinaryNode node) {
		// Base case - reach end of tree, key is not found
		if(node == null) {
			return 0;
		}

		// Base case - Found our target key
		if(node.key.equals(key)) {
			return node.value;
		}

		// Compare the search key to the current node's key to determine traversal
		int comparison = key.compareTo(node.key);
		// Search the right subtree
		if(comparison > 0) {
			return occurrences (key, node.right);
		}
		// Search the left subtree
		else {
			return occurrences(key, node.left);
		}
	}


	public java.util.Set keys() {
		java.util.Set<String> result = new java.util.HashSet<>();

		return keys(result, root);
	}


	// keys() helper method, using recursion 
	private java.util.Set keys(java.util.Set set, BinaryNode node) {

		if (node == null) {
			return set;
		}
		else {
			set.add(node.key);
			keys(set, node.left);
			keys(set, node.right);
		}
		return set;
	}


	public void add(String key) {
		root = add(key, root);
	}

	// add() helper method, using recursion
	protected BinaryNode add(String key, BinaryNode node) {
		// If key doesn't exist, add new node with the key, w/ initial count of 1
		if (node == null) {
			return new BinaryNode(key, 1);
		}

		// If the key already exist in the tree, increase it's count by 1
		if(key.compareTo(node.key) == 0) {
			node.value++;
		}
		// If we haven't found a matching key, continue our traversal
		else { 
			if (key.compareTo(node.key) < 0) { // add to left subtree
				node.left = add(key, node.left);
			}
			else {  // add to right subtree
				node.right = add(key, node.right);
			}
		}
		return node;
	}


	/**
	 * Removes one occurrence of the given key from the tree, usually
	 * by decrementing the value associated with the key.
	 * However, if the key has a value of 1, deletes the node
	 * If the key is not in the tree, does nothing.
	 * @param key
	 */
	public void removeOne(String key) {
		//TODO: search the tree for the target key.
		// If found, decrease the count by 1.
		// If the resulting count (after decreasing it) is 0, remove that node

		removeOne(key, root);


	}

	protected BinaryNode removeOne(String key, BinaryNode node) {
		if (node == null) {	// key not in tree
			return null;
		}
		if (key.compareTo(node.key) == 0) { // remove this node or decrement
			if (node.value == 1) { // If the vlaue is one then delete node
				if (node.left == null) { // replace this node with right child
					return node.right;
				} else if (node.right == null) { // replace with left child
					return node.left;
				} else {
					// replace the value in this  node with value in the
					// rightmost node of the left subtree
					BinaryNode replacement = getRightmost(node.left);

					// now remove the rightmost node in the left subtree,
					// by calling ourselves recursively
					BinaryNode newNode = new BinaryNode(replacement.key, replacement.value, remove(replacement.key, node.left), node.right);
					return newNode;
				}
			}
			else { // if the value is not 1 then just decrement value
				node.value--;
			}

		} else {		// remove from left or right subtree
			if (key.compareTo(node.key) < 0) {
				// remove from left subtree
				node.left = removeOne(key, node.left);
			} else {		// remove from right subtree
				node.right = removeOne(key, node.right);
			}
		}
		return node;
	}


	/**
	 * Removes the given String from the tree by deleting the node
	 * @param key
	 */
	public void remove(String key) {

		root = remove(key, root);
	}


	// remove() recursive helper method
	protected BinaryNode remove(String key, BinaryNode node) {
		if (node == null) {	// key not in tree
			return null;
		}
		if (key.compareTo(node.key) == 0) { // remove this node
			if (node.left == null) { // replace this node with right child
				return node.right;
			} else if (node.right == null) { // replace with left child
				return node.left;
			} else {
				// replace the value in this  node with value in the
				// rightmost node of the left subtree
				BinaryNode replacement = getRightmost(node.left);

				// now remove the rightmost node in the left subtree,
				// by calling ourselves recursively
				BinaryNode newNode = new BinaryNode(replacement.key, replacement.value, remove(replacement.key, node.left), node.right);
				return newNode;
			}
		} else {		// remove from left or right subtree
			if (key.compareTo(node.key) < 0) {
				// remove from left subtree
				node.left = remove(key, node.left);
			} else {		// remove from right subtree
				node.right = remove(key, node.right);
			}
		}
		return node;
	}


	// return the right-most node in the subtree
	protected BinaryNode getRightmost(BinaryNode node) {
		assert(node != null);
		BinaryNode right = node.right;
		if (right == null) {
			return node;
		} else {
			return getRightmost(right);
		}
	}

	
	/**
	 * 
	 * @return number of nodes in the tree == the number of unique Strings
	 */
	public int size() {

		return size(root);
	}

	// size() helper method
	private int size(BinaryNode node) {
		// Base case
		if(node == null) return 0;
		return 1 + size(node.left) + size(node.right);
	}
	

	/**
	 * 
	 * @return the height of the tree.  An empty tree has height 0,
	 *         a tree with one node has height 1
	 */
	public int height() {
		
		return height(root);
	}
	
	// height() helper method
	private int height(BinaryNode node) {

		if (node == null) {
			return 0;
		}
		else {
			int rightSubtreeDepth = height(node.right);
			int leftSubtreeDepth = height(node.left);

			if (leftSubtreeDepth > rightSubtreeDepth) {
				return (leftSubtreeDepth + 1);
			}
			else {
				return(rightSubtreeDepth + 1);
			}
		}

	}
	

	private class BSTComparator implements Comparator<String>{
		@ Override
		public int compare(String o1, String o2) {
			return (int) (occurrences(o2) - occurrences(o1));
		}
	}

	
	public void printTop10() {

		Set<String> set = keys();

		String[] keys = new String[set.size()];
		int index = 0;

		for(String key : set) {
			keys[index++] = key;
		}
		
		Arrays.sort(keys, new BSTComparator());

		for(int i = 0; i < 10; i++) {
			System.out.println(keys[i] + ": " + occurrences(keys[i]));
		}
	}

	
	public static void main(String[] args) throws FileNotFoundException{

		BinaryStringTree constitution = new BinaryStringTree("constitution.txt");
		
		constitution.printTop10();
		System.out.println(constitution.height());
		System.out.println(constitution.size());
	}
	
	// ~Analysis~
	/*
	 * the height of my tree is 24 and comparing to the minimum possible height of a 
	 * perfectly balanced tree which is 11, I would say my tree is not balanced nor
	 * is it too unbalanced. if the tree is really unbalanced then it would have the 
	 * maximum height of 1265. I can guarantee that all the values in the tree are at 
	 * least 1 because my implementation for the add, remove and removeOne method are 
	 * correct. for the add method every single time a node is added, if it's a node 
	 * that's not yet in the tree then a new node is created with a value of 1 and if 
	 * the node is found in the tree then the value is incremented. this guarantees 
	 * all new node created will have value of at least one. for my remove method, 
	 * if the node we want to remove is not in the tree then nothing happens, if it
	 * is in the tree then that node will be removed, if the node has two children then
	 * the right-most node of the left subtree will take place of the node that's being 
	 * removed. Same for removeOne method, but if the node we want to remove is in the 
	 * tree then we will just decrement the value, if the value of node we want to remove 
	 * is 1 then we will delete that node entirely making sure no node in tree have value 
	 * less than 1.
	 */

}

```
