
#include <iostream>

namespace graph{
using namespace std;
class Node{
    private:
    int name;   
    int index; 
    int* weight;
    Node** son;
    int value; // for the algorithmes
    public:

    Node(){
        weight=NULL;
        son=NULL;
        index=0;
        name=0;
        value=__INT_MAX__;
    }
    

    void setNode(int length,int id) {
        weight=new int[length];
        son = new Node*[length];
        name=id;

    }
    int getValue(){
        return value;
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

    int getName(){
        return name;
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
            std::cout<<"->" << son[i]->getName()<<"->";
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

        void del(int v,int u){
            (arr[v]).delSon(&arr[u]);
            arr[u].delSon(&arr[v]);

        }

        void printGrapgh(){
            for (int i = 0; i <num; i++)
            {
                arr[i].printNode();
                cout<<""<<endl;
            }
            
            

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
        Node* getTop(){
            return arr[index];
        }

        Node* remove(){
            Node* r=arr[index];
            delete arr[index];
            return r;
        }
        void printQ(){
            for (int i = 0; i < index; i++)
            {
                cout<<arr[i]->getName()<<endl;
            }
            cout<<"done loop"<<endl;
            
        }

        int getMin(){
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
            return minValue;
        }

    };

    }
    
    int main(){ 
    using namespace graph;
    using namespace ququ;
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
    A.add(5,3,5);
    A.add(0,4,5);
    A.add(4,5,5);
    A.printGrapgh();

    
    
    
    }
