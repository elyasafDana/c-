
#include <iostream>

namespace graph{
using namespace std;
class Node{
    private:
    int name;   
    bool white;
    int index;// num of sons
    int* weight;
    Node** son;
    int value; // for the algorithmes
    public:

    Node(){
        weight=NULL;
        son=NULL;
        index=0;// num of sons
        name=0;
        value=__INT_MAX__;
        white=true;
    }
    

    void setNode(int length,int id) {
        weight=new int[length];
        son = new Node*[length];
        name=id;
        white=true;
        index=0;

    }
    int getNumOfSons(){
        return index;
    }
    
    int getName(){
        return name;
    }
    Node** getSons(){
        return son;
    }
    int* getWeight(){
        return weight;
    }
    int getValue(){
        return value;
    }
    bool isWhite(){
        return white;
    }
    void setColor(bool color){
        white=color;
    }
    void setValue(int v){
        value=v;
    }

    void addSon( Node* v, int w){
        son[index]=v;
        weight[index]=w;
        index++;
    }
    void addSon( Node* v){
        son[index]=v;
        weight[index]=1;
        index++;
    }

    


    void delSon(Node* v){
        int delNum= (*v).getName();
        bool found=false;
        for(int i=0;i<index;i++){
            if ((*son)[i].getName()==delNum )
            {
                found=true;
                delete son[i];
                 weight[i]=0;
                while (i<index)
                {
                   son[i]=son[i+1];
                   weight[i]=weight[i+1];
                }
                index--;
                
            }
            
        }
        if (!found)
        {
            //here we need to add error
        }
    

    }

    void printNode(){
        std::cout<<name;
        for(int i=0;i<index;i++){
            std::cout<< ","<<son[i]->getName()<<",";
        }
    }


};
    class Graph{
        private:
        Node* arr;
        const int num;
    
        public:
    
        Graph(int x): num(x){

            arr = new Node[x];
            for (int i = 0; i < x; i++)
            {
               arr[i].setNode(x,i);
            }
            
        }  //restart list bacuse of the const

        void add(int v,int u,int w){
            arr[v].addSon(&arr[u],w);
            arr[u].addSon(&arr[v],w);
        }
        void add(int v,int u){
            arr[v].addSon(&arr[u],1);
            arr[u].addSon(&arr[v],1);
        }

        void del(int v,int u){
            (arr[v]).delSon(&arr[u]);
            arr[u].delSon(&arr[v]);

        }

        void printGrapgh(){
            for (int i = 0; i <num; i++)
            {
                cout<<i<<":";
                arr[i].printNode();
                cout<<endl;
            }
            
            

        }

        int getNumOfNode(){
            return num;
        }
        Node* getNodeAtIndex(int index){
            return &(arr[index]);
        }
    
    
    };
    
    
    
    }

    namespace ququ{
        using namespace graph;

        class Ququ{
        private:
            Node** arr;
            int index;
            int arrSize;
            
        public:
        Ququ(){
            arr=new Node*[5];
            index=0;
            arrSize=5;
        }

        Ququ(Node* n){
            arr=new Node*[5];
            index=1;
            arr[0]=n;
            arrSize=5;
        }
        int getIndex(){
            return index;
        }

        void add(Node* n){
            if(index==arrSize){
                arrSize=arrSize*2;
                Node** tempArr=new Node*[arrSize];
                for (int i = 0; i < index; i++)
                {
                        tempArr[i]=arr[i];
                        delete arr[i];
                }
                arr=tempArr;
            }
                arr[index]=n;
                index++;
            

        }
        Node* getFirst(){
            cout<<"getFirst"<<endl;

            return remove(0);
            
        }

        Node* remove(int x){
            Node* r=arr[x];
            cout<<x<<"   "<<index<<endl;
            
        
            while (x<index && x!=arrSize&& arr[x]!=NULL)
            {
                arr[x]=NULL;
                cout<<"deleting"<<x<<endl;
                arr[x]=arr[x+1];
                cout<<"deleting"<<x<<endl;
                x++;
            }  
            if (arr[x]!=NULL)
            {
                cout<<"de";
                 arr[x]=NULL;

            }
            cout<<"asdas";
            index--;      
            cout<<"done"<<x<<endl;
            return r;
        }
        void printQ(){
            for (int i = 0; i < index; i++)
            {
                cout<<arr[i]->getName()<<endl;
            }
            cout<<"done loop"<<endl;
            
        }
        bool isEmpty(){
            if (index==0)
            {
                return true;
            }
            return false;
        }

        Node* getMin(){
            int minValue=__INT_MAX__;
            Node* min;
            int minIndex=0;
            for (int i = 0; i < index; i++)
            {
                if(arr[i]->getValue()<minValue){
                    minValue=arr[i]->getValue();
                    min=arr[i];
                    minIndex=i;
                }

            }
            while(minIndex<index-1){
                delete arr[minIndex];
                arr[minIndex]=arr[minIndex+1];
                minIndex=minIndex+1;
            }
            delete arr[minIndex];
            return min;
        }

    };

    }

    namespace Algorithms{
        using namespace graph;
        using namespace ququ;
        using namespace std;

        Graph bfs(Graph g,Node* n){
            Graph rg= Graph(g.getNumOfNode());
            Ququ q= Ququ();
            for (int  i = 0; i <g.getNumOfNode() ; i++)
            {

                g.getNodeAtIndex(i)->setColor(true);
            }
            g.getNodeAtIndex(n->getName())->setColor(false);
            q.add(g.getNodeAtIndex(n->getName()));

            while (!q.isEmpty())
            {
                cout<<"into the while"<<endl;
                Node* n= q.getFirst();
                cout<<"keep going"<<endl;
                Node** sons=n->getSons();
                for (int i = 0; i < n->getNumOfSons(); i++)
                {
                    if (sons[i]->isWhite())
                    {
                        rg.add(n->getName(),sons[i]->getName());
                        sons[i]->setColor(false);
                        q.add(sons[i]);
                    }
                }
                
            }
            return rg;
        }

        void dfsVISIT(Node* n,Graph* rg){

            cout<<"visit"<<endl;
            n->setColor(false);

            Node** sons=n->getSons();
            for (int i = 0; i < n->getNumOfSons(); i++)
            {
                if (sons[i]->isWhite())
                {
                    cout<<"adding"<<n->getName()<<","<<sons[i]->getName();
                    rg->add(n->getName(),sons[i]->getName());
                    dfsVISIT(sons[i],rg);
                }
                sons[i]->setColor(false);
            }
            
        } 
        Graph DFS(Graph g,Node* n){
            
            for (int  i = 0; i <g.getNumOfNode() ; i++)
            {
                g.getNodeAtIndex(i)->setColor(true);
            }
            Graph rg=Graph(g.getNumOfNode());
            Node** sons=n->getSons();

           

            


            for (int i = 0; i < g.getNumOfNode(); i++)
            {
                sons=g.getNodeAtIndex(i)->getSons();
                cout<<"inside for"<<endl;
                if (sons[i]!=NULL&&sons[i]->isWhite())
                {
                    cout<<"inside "<<i<<endl;

                    dfsVISIT(sons[i],&rg);
                }
                
            }
            return rg;

        }

        

    };
    
    int main(){ 
    using namespace graph;
    using namespace ququ;
    using namespace Algorithms;
    
    Ququ* b=new Ququ();
    Node* n1= new Node();
    n1->setValue(1);
    n1->setNode(1,1);
    Node* n2=new Node();
    n2->setValue(2);
    n2->setNode(2,2);
    Node* n3=new Node();
    n3->setValue(3);
    n3->setNode(3,3);
    
    
    

    b->add(n3);
    b->add(n2);
    b->add(n1);
    cout<<"index="<<b->getIndex()<<"printing Q:"<<endl;
    b->printQ();
    //cout<<n1->getValue()<<n2->getValue()<<n3->getValue()<<endl;

    cout<<"MONIMU,"<<b->getMin()<<endl;

    Graph A= Graph(8);
    A.add(1,2);
    A.add(1,5);
    A.add(1,4);
    A.add(2,3);
    A.add(3,4);
    A.add(4,7);
    A.add(7,5);
    A.add(5,6);
    A.printGrapgh();
    Graph B=bfs(A,A.getNodeAtIndex(1));
    cout<<"this is B:"<<endl;
    B.printGrapgh();

    cout<<"this is c:"<<endl;

    Graph c=DFS(A,A.getNodeAtIndex(0));
    c.printGrapgh();
    
    
    
    }
