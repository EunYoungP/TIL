
# __[프로그래머스 LV3] 길찾기 게임__

트리 순회

----

## __문제__

[길찾기 게임](https://programmers.co.kr/skill_checks/482003?challenge_id=2489)<br><Br>

## __나의 풀이__
```c++
#include <string>
#include <vector>

using namespace std;
typedef struct node *nodePointer;
typedef struct node
{
    int data;
    nodePointer left, right;
};

// 전위순회
void PreOrder(nodePointer nodepointer, vector<int> &answerVector)
{
    if(nodepointer)
    {
        answerVector[0].push_back(nodepointer->data);
        
        PreOrder(nodepointer->left, answerVector);
        PreOrder(nodepointer->right, answerVector);
    }
}

// 후위순회
void PastOrder(nodePointer nodepointer, vector<int> &answerVector)
{
    if(nodepointer)
    {
        PastOrder(nodepointer->left, answerVector);
        PastOrder(nodepointer->right, answerVector);
        answerVector[1].push_back(nodepointer->data);
    }
}

// 각 노드의 좌표가 1번 노드부터 순서대로 저장된 2차원 배열 (1 <= 10000)
vector<vector<int>> solution(vector<vector<int>> nodeinfo) {
    vector<vector<int>> answer;
    
    vector<node> nodes(10001);
    // 전우 순회, 후위 순회 결과
    
    for(int i =0; i < nodeinfo.size(); i++)
    {
        for(int j= 0; j < nodeinfo[i].size(); j++)
        {
            nodes[i].data = nodeinfo[i][j];
            nodes[i].left = NULL;
            nodes[i].right = NULL;
        }
    }
    
    return answer;
}
```

left, right, data를 가진 노드를 생성하여 트리를 만들어 후위순회, 전위순회를 하려고 했습니다.

이 과정에서 이진트리가 아닌 트리를 정렬하는 부분에서 막혀 문서를 참고했습니다.

이진 트리라면 순서대로 정렬하여 짝수인 위치의 노드를 left 아닌 노드를 right로 설정해주면 됩니다.<br><Br>


## __참고 풀이__
```C++
#include <string>
#include <vector>
#include <algorithm>

using namespace std;
struct node
{
    int data, x, y;
    node *left, *right;
};

// 노드들을  y, x값 우선수위에 따라 정렬합니다.
bool comp(struct node a, struct node b)
{
    if(a.y < b.y)
    {
        if(a.y == b.y)
        {
            if(a.x < b.x)
            {
                return true;
            }
            return false;
        }
        return true;
    }
    return false;
}

// 정렬된 노드들을 이용하여 이진트리를 만듭니다.
void MakeBinaryTree(node *root, node* child)
{
    // 루트 노드보다 x값이 작다면 왼쪽 자식 노드로 설정합니다.
    if(root->x > child->x)
    {
        if(root->left == NULL)
        {
            root->left = child;
            return;
        }
        // 이미 왼쪽 자식 노드가 차있다면 더 아래 차수의 자식 노드로 설정합니다.
        MakeBinaryTree(root->left, child);
    }
    // 루트 노드보다 x값이 크다면 오른쪽 자식 노드로 설정합니다.
    else
    {
        if(root->right == NULL)
        {
            root->right = child;
            return;
        }
        MakeBinaryTree(root->right, child);
    }
}

// 전위순회
void PreOrder(node *nodepointer, vector<int> &answerVector)
{
    if(nodepointer)
    {
        // root, left, right 순으로 순회합니다.
        answerVector.push_back(nodepointer->data);
        
        PreOrder(nodepointer->left, answerVector);
        PreOrder(nodepointer->right, answerVector);
    }
}

// 후위순회
void PastOrder(node *nodepointer, vector<int> &answerVector)
{
    if(nodepointer)
    {
        // left, right, root 순으로 순회합니다.
        PastOrder(nodepointer->left, answerVector);
        PastOrder(nodepointer->right, answerVector);
        answerVector.push_back(nodepointer->data);
    }
}

// 각 노드의 좌표가 1번 노드부터 순서대로 저장된 2차원 배열 (1 <= 10000)
vector<vector<int>> solution(vector<vector<int>> nodeinfo) {
    vector<vector<int>> answer(2,vector<int>(nodeinfo.size()));
    
    vector<node> Tree(nodeinfo.size());
    // 전우 순회, 후위 순회 결과
    
    for(int i= 0; i < nodeinfo.size(); i++)
    {
        Tree[i].data = i+1;
        Tree[i].x = nodeinfo[i][0];
        Tree[i].y = nodeinfo[i][1];
        Tree[i].left = NULL;
        Tree[i].right = NULL;
    }
    
    sort(Tree.begin(), Tree.end(), comp);
    node *root = &Tree[0];
    for(int i = 1; i < Tree.size(); i++)
    {
        MakeBinaryTree(root, &Tree[i]);
    }
    
    vector<int> preVec; PreOrder(root, preVec);
    vector<int> pastVec; PreOrder(root, pastVec);
    answer[0] = preVec;
    answer[1] = pastVec;
    return answer;
}
```

