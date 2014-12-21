NLP_coursera_basic-cod
======================
# h1 part1 3 different code for each 

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
            
            
            

# h1 part 2 only code in one file 


def prob(yi2,yi1,yi):

    qx =0
    qy =0
    f = open('new_counts')
    l = True
    while l:
        l = f.readline()

        line = l.strip()

        if line:

            fd = line.split()

            if fd[1] == '3-GRAM' and fd[2] == yi2 and fd[3] == yi1 and fd[4] == yi:
                qx = float(fd[0])

            if fd[1] == '2-GRAM' and fd[2] == yi2 and fd[3] == yi1:
                qy = float(fd[0])
    
    
    return qx/qy


def emission(y,x):
    l=0
    ey=1
    ex=0
    hand = open('new_counts')

    for iine in hand:

        fd = iine.strip().split(" ")
        
        
        if fd[1] == 'WORDTAG' and fd[2] == x and fd[3] == y:
            ex = float(fd[0])
        elif fd[1] == 'WORDTAG' and fd[2] == x and fd[3] == '_RARE_':
            ex =float(fd[0])
            
        if fd[1] == '1-GRAM' and fd[2]== x:
            ey = float(fd[0])
    #print ex/ey
    return ex/ey



def argmaxy(sen):
    s =[]
    x = sen.split()
    n = len(x)
    tag =0.0
    u1 = ''
    v1 = ''
    y =[]
    
    s.append('*')
    for c in range(1,n): 
        s.append(['O','I-GENE'])
    s.append('*')
    
    pi = []
    
    pi = {'ran': range(0,n),'pre': s[:],'post': s[:]}
    bp = {'ran': range(0,n),'pre': s[:],'post': s[:]}
    pi[0,'*','*']= 1

    
    for k in range(1,n+1):
        p = 0.0 
        for (u,v) in [(u,v) for u in s[k-1] for v in s[k]]:
            for w in s[k-2]:
                
                try:
                    tag = pi[k-1,w,u]*prob(w,u,v)*emission(x[k-1],v)
                    if p < tag :
                        p = tag
                        val =w
                        u1 = u
                        v1= v
                        
                        
                except:
                    
                    #print k,w,u,v,'prob(w,u,v)',prob(w,u,v),'emission(x[k-1],v1)',x[k-1],v1,emission(x[k-1],v1),'tag',tag,'pi[k-1,w,u1]',pi[k-1,w,u1]
                    continue
        pi[k,u1,v1] = p
        bp[k,u1,v1] = val
        if bp[k,u1,v1] != '*':
            print bp[k,u1,v1] 
        #print 'k',k,'u1',u1,'v1',v1,'w1',val,'prob(val,u1,v1)',prob(val,u1,v1),'emission(x[k-1],v)',x[k-1],v,emission(x[k-1],v1),'p',p,'tag',tag,'pi[k-1,w1,u1]',pi[k-1,val,u1],'bp[k,u,v]',bp[k,u1,v1]
   
    for (u,v) in [(u,v) for u in s[n-1] for v in s[n]]:
        m =0
        try:
            tag = pi[n,u,v]*prob(u,v,'STOP')
            if m < tag:
                m = tag
                u1 =u
                v1 = v
                
        except:
            continue
    
    print u1
    print v1
    #for k in range(n-2,0,-1):
        #y.insert(k,(bp[k+2,y[k+1],y[k+2]]))
    #y.append(u)
    #y.append(v)

    

    

def calling():
    f = open('gene.test')
    l = True
    x = ''

    while l:

        l = f.readline()
        line = l.strip()
        if line:
            x = x+' '+line
        else :
            u = argmaxy(x)
            x = ''
            print

calling()




# h1 part 3 code in one file some changes has done in basic rare code 







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
                    if hasNumbers(fld[0]):
                        print 'Numeric'+" " + fld[1]
                    elif hasallcap(fld[0]):
                        print 'All Capitals '+ " "+ fld[1]
                    elif haslastcap(fld[0]):
                        print 'Last Capital'+ " "+ fld[1]
                    else:
                        print 'RARE'+" " + fld[1]        
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

          

def hasNumbers(inputString):
    return any(char.isdigit() for char in inputString)
def hasallcap(inputstring):
    return inputstring.isupper()

def haslastcap(inputstring):
    l = len(inputstring)
    char = inputstring[l-1]
    return char.isupper()

rare()












            
        












    


    



















            
        
    
    

    
            
            




