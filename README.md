NLP_coursera_basic-cod
======================
# this file contain 3 different code for part 1  H1

#count emission
def emission(x,y):
    l=0
    ey=1
    ex=0
    hand = open('new_counts')

    for iine in hand:

        fd = iine.strip().split(" ")
        
        
        if fd[1] == 'WORDTAG' and fd[2] == x and fd[3] == y:
            ex = float(fd[0])
        if fd[1] == '1-GRAM' and fd[2]== x:
            ey = float(fd[0])
        
    return ex/ey
def call():
    l=0
    hand = open('new_counts')

    for line in hand:
        fd = line.strip().split(" ")
        
        if fd[1] == 'WORDTAG':
            print fd[2],fd[3],emission(fd[2],fd[3])
call()

#put rare

def rare():
    count = 0

    f = open('gene.train')
    l= True

    while l:


        l = f.readline()
        line = l.strip()

        if line :

            fld =line.split()
            k = fld[0]
            count = counts(fld[0])
            try:
            
        
                if count >=5:
                    print fld[0] +" "+ fld[1]
                else:
                    print '_RARE_'+" " + fld[1]        
            except:
                continue

        else:
            print 


def counts(x):
    f = open('gene.train')
    l= True
    count = 0
   
    while l:


        l = f.readline()
        line = l.strip()

        if line :

            fld =line.split()
            try:
                if fld[0] == x:
                    count = count +1
            except:
                continue
            if count > 5:
                return 100
    return count
rare()


#find max




def find_next():

    f = open('gene.test')
    l = True
    while l:

        l = f.readline()
        line = l.strip()

        if line:
            
            tag = find_max(line)

            print line , tag
        else:
            print


def find_max(x):
    f = open('emission')
    g = open('emission')
    l = True
    m = True
    num = 0
    tag = 'll'
    while l:
        
        l = f.readline()
        line = l.strip()

        fd = line.split()
        
        try:
            if fd[1] == x:
                if num < float(fd[2]):
                    num = float(fd[2])
                    tag = fd[0]

        except:
            continue
        
    if tag == 'll':
        
        while m:
            m = g.readline()
            line1 = m.strip()
            fd = line1.split()
          
            try:
                if fd[1] == '_RARE_':
                    
                    if num < float(fd[2]):
                        num = float(fd[2])
                        tag = fd[0]

            except:
                continue
    return tag
        

find_next()
            
            
            


    
            
            




