# Lab2LinkedL

'''
Elizabeth Rubio
Lab 2
'''

import copy

#constructors
class Node(object):
   item = -1
   next = None

   def __init__(self, item, next):
       self.item = item
       self.next = next
       
class List(object):
    head = None
    tail = None
    
    def __init__(self):
        self.head = None
        self.tail = None
        
    def get_index_of(self, item):
        counter = 0
        temp = self.head
        while temp is not None:
            if temp.item == item:
                return counter
            counter += 1
            temp = temp.next
        return -1

    def remove(self, index):
        temp = self.head
        if index < 0:
            return
        if index == 0:
            if self.head is not None:
                self.head = self.head.next
                return
        for i in range(index-1):
            if temp is None:
                return None
            temp = temp.next
        if temp is not None and temp.next is not None:
            temp.next = temp.next.next 
            
    def size(self):
        counter = 0
        temp = self.head
        while temp is not None:
            counter += 1
            temp = temp.next
        return counter

#Reading file and link list made
def fileReading(file, LList):
    textOpener = open(file, 'r')
    readFile = textOpener.read().splitlines()
    textOpener.close()
    returnLL = arrayToLL(readFile, LList)
    return returnLL

#psrt of the fileReading method to turn the list into a linked list
def arrayToLL(arr, LL):
    for l in range(len(arr)):
        if LL.head is None:
            LL.head = Node(arr[l], None)
            LL.tail = LL.head
        else:
            LL.tail.next = Node(arr[l], None)
            LL.tail = LL.tail.next
    return LL

#The inner loop checks if an of the items match with the outer loop
def nestedSolution(LList):
    temp1 = LList.head
    temp2 = LList.head.next
    counter = 0
    while temp1.next is not None:
        while temp2 is not None:
            if temp1.item == temp2.item:
                print(temp1.item, " is a duplicate")
                counter += 1
            temp2 = temp2.next
        temp1 = temp1.next
        temp2 = temp1.next
    return counter

#The inner loop will compare the item and the following one then based of the list
#the first changes have been made it will then store it into a new linked list
#then it will exit the inner loop and execute then following one which then checks
#if they are all in order. Otherwise the loop will continue, and will empy the new
#linked list until they are all in order
def bubbleSort(LList):
    lengthLL = length(LList)
    temp1 = LList.head
    temp2 = LList.head
    newLL = List()
    temp3 = None
    while temp1 is not None:
        while temp2 is not None:
            if temp2.next is None:
                newLL.tail.next = temp2
                break
            if int(temp2.item) > int(temp2.next.item):
                temp2.item, temp2.next.item = temp2.next.item, temp2.item
            if newLL.head is None:
                newLL.head = Node(temp2.item, None)
                newLL.tail = newLL.head
            else:
                newLL.tail.next = Node(temp2.item, None)
                newLL.tail = newLL.tail.next
            temp2 = temp2.next
        temp3 = newLL.head
        counter = 0
        while temp3.next is not None:
            if int(temp3.item) <= int(temp3.next.item):
                counter += 1
            temp3 = temp3.next
        if counter == lengthLL-1:
            return newLL
        temp2 = copy.deepcopy(newLL.head)
        newLL.head = None
        temp1 = temp1.next
    return newLL

#This method only checks if there are duplicated for bubble sort and merge sort
def checkDup(LL):
    temp = LL.head
    counter = 0 
    while temp.next is not None:
        if int(temp.item) == int(temp.next.item):
            print(temp.item, " is a duplicate")
            counter += 1
        temp = temp.next
    return counter
   
#calcuates the length of the linked list
def length(LL):
    temp = LL.head
    counter = 0
    while temp is not None:
        counter +=1
        temp = temp.next
    return counter

#for the mergesort function the middle is calculated then the while loop will
#store both of them into either the left or right
def middle(LL):
    leng = length(LL)
    mid = leng//2
    leftLL = List()
    rightLL = List()
    temp = LL.head
    counter = 0
    while temp is not None:
        if counter < mid:
            if leftLL.head is None:
                leftLL.head = Node(temp.item, None)
                leftLL.tail = leftLL.head
            else:
                leftLL.tail.next = Node(temp.item, None)
                leftLL.tail = leftLL.tail.next
        if counter >= mid:
            if rightLL.head is None:
                rightLL.head = Node(temp.item, None)
                rightLL.tail = rightLL.head
            else:
                rightLL.tail.next = Node(temp.item, None)
                rightLL.tail = rightLL.tail.next
        counter += 1
        temp = temp.next
    return leftLL,rightLL

#this method compares the items in both lists and will then store it into a new
#linked list in the correct order
def merge2LL(LL1, LL2, newLL):
    if int(LL1.head.item) > int(LL2.head.item):
        if newLL.head is None:
            newLL.head = Node(LL2.head.item, None)
            newLL.tail = newLL.head
        else:
            newLL.tail.next = Node(LL2.head.item, None)
            newLL.tail = newLL.tail.next
        newLL.tail.next = Node(LL1.head.item, None)
        newLL.tail = newLL.tail.next
    else:
        if newLL.head is None:
            newLL.head = Node(LL1.head.item, None)
            newLL.tail = newLL.head
        else:
            newLL.tail.next = Node(LL1.head.item, None)
            newLL.tail = newLL.tail.next
        newLL.tail.next = Node(LL2.head.item, None)
        newLL.tail = newLL.tail.next
    return newLL

#this function is the one working recursively and is the one that keeps calling
#middle to keep splitting the method into smaller sections
def mergeSort(LL):
    if LL.head == None:
        return LL
    if LL.head.next == None:
        return LL
    leftLL, rightLL = middle(LL)
    left = mergeSort(leftLL)
    right = mergeSort(rightLL)
    finalSort = List()
    finalSort = merge2LL(left, right, finalSort)
    return finalSort 

#Because the method above only combine single items this method will combine each
#new ordered pair and include them in a single linked list and remove them from
#the original one and keep getting the smallest two from the list. It will keep
#going until it is in order
def completeMerge(LL):
    removeLL = copy.deepcopy(LL)
    newLL = List()
    while removeLL is not None:
        tempLL = mergeSort(removeLL)
        if tempLL.size() <= 0:
            return newLL
        tempNode = tempLL.head
        while tempNode is not None:
            if newLL.head is None:
                newLL.head = Node(tempNode.item, None)
                newLL.tail = newLL.head
            else:
                newLL.tail.next = Node(tempNode.item, None)
                newLL.tail = newLL.tail.next
            index = removeLL.get_index_of(tempNode.item)
            removeLL.remove(index)
            tempNode = tempNode.next  
 
#Getting the highest possible number then creating a boolean list with the amount
#of numbers up to the highest. Each one is set to false and if there happens to
#be an index that has one as true then it is a duplicate.
def singlePass(LL):
    temp1 = LL.head
    temp2 = LL.head
    maxx = 0
    counter = 0
    while temp1 is not None:
        if int(temp1.item) > maxx:
            maxx = int(temp1.item)+1
        temp1 = temp1.next
    List = [False]*maxx
    while temp2 is not None:
        index = int(temp2.item)
        if List[index] == False:
            List[index] = True
        elif List[index] == True:
            print(temp2.item, " is a duplicate")
            counter += 1
        temp2 = temp2.next
    return counter

#This was used to visualize and implement the functions
def Print(F):
    temp = F.head
    counter = 0
    while temp is not None:
        counter += 1
        print(temp.item, end=' ')
        temp = temp.next
    print("this is the counter:", counter)
    print()

#The user picks which option they want
def main():
    LL = List()
    LL = fileReading('activision.txt', LL)
    completeLL = fileReading('vivendi.txt', LL)
    print("Select solution from 1 to 4:", end = '')
    option = int(input())
    if option == 1:
        numDuplicates = nestedSolution(completeLL)
        print("Number of duplicates: ", end='')
        print(numDuplicates)
        return
    if option == 2:
        LinkedList = bubbleSort(completeLL)
        numDuplicates = checkDup(LinkedList)
        print("Number of duplicates: ", end='')
        print(numDuplicates)
        return
    if option == 3:
        LinkedList = completeMerge(completeLL)
        numDuplicates = checkDup(LinkedList)
        print("Number of duplicates: ", end='')
        print(numDuplicates)
        return
    if option == 4:
        counter = singlePass(completeLL)
        print("Number of duplicates: ", counter)
        return
    else:
        print("Invalid input")
        return

main()
