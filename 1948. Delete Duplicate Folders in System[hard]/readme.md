# [1948. Delete Duplicate Folders in System](https://leetcode.com/problems/delete-duplicate-folders-in-system/)

## 一、题目描述
>Due to a bug, there are many duplicate folders in a file system. You are given a 2D array `paths`, where `paths[i]` is an array representing an absolute path to the `ith` folder in the file system.
>
>- For example, `["one", "two", "three"]` represents the path `"/one/two/three"`.
>
>Two folders (not necessarily on the same level) are **identical** if they contain the **same non-empty** set of identical subfolders and underlying subfolder structure. The folders **do not** need to be at the root level to be identical. If two or more folders are **identical**, then **mark** the folders as well as all their subfolders.
>
>- For example, folders `"/a"` and `"/b"` in the file structure below are identical. They (as well as their subfolders) should all be marked:
>  - `/a`
>  - `/a/x`
>  - `/a/x/y`
>  - `/a/z`
>  - `/b`
>  - `/b/x`
>  - `/b/x/y`
>  - `/b/z`
>- However, if the file structure also included the path `"/b/w"`, then the folders `"/a"` and `"/b"` would not be identical. Note that `"/a/x"` and `"/b/x"` would still be considered identical even with the added folder.
>
>Once all the identical folders and their subfolders have been marked, the file system will **delete** all of them. The file system only runs the deletion once, so any folders that become identical after the initial deletion are not deleted.
>
>Return *the 2D array* `ans` *containing the paths of the **remaining** folders after deleting all the marked folders. The paths may be returned in **any** order*.

## 二、用到的容器

- **解法一(maijiko):**

    ``` c++
    std::map, std::unordered_map, std::vector
    ```

## 三、提交结果

- **解法一:**

    ![解法一](pics/solution_1_pass.png)

## 四、解题思路

- **解法一:**

    先构建目录树，然后用 DFS(深度优先遍历) 计算每个节点的哈希值，将重复的哈希值标记，最后再用一次 DFS 收集目录
    
    step 1. 创建根节点后，根据 paths 生成目录树
    
    step 2. 采用 DFS 的方式生成以当前目录为根目录的签名(哈希值)，并将签名存储在 **签名 - 相同签名数量** 的 `std::unordered_map` 中，签名格式: 当前节点的名字并且以 `#` 结尾
    
    step 3. 遍历每个节点，查找是否有重复的签名
    
    - 存在重复签名: 标记当前节点删除标记为 `true`
    - 不存在重复签名: 继续遍历当前节点的子节点
    
    step 4. 根据 **节点删除标记** 内容将没有删除的节点存储，这一步需要一个中间变量用来存储当前遍历节点的绝对路径，在遍历子节点 的时候，需要回溯绝对路径

## 五、代码实现

- **解法一:**

    ``` c++
    class Solution {
    public:
        struct FolderNode {
            std::string name;
            std::map<std::string, FolderNode*> children;
            std::string hash;
            bool toDelete = false;
            FolderNode(std::string n) : name(n) {}
        };
    
        vector<vector<string>> deleteDuplicateFolder(vector<vector<string>>& paths) {
            FolderNode* root = new FolderNode("");
            for (const auto& path : paths) {
                FolderNode* current = root;
                for (const string& folder : path) {
                    if (current->children.find(folder) == current->children.end()) {
                        current->children[folder] = new FolderNode(folder);
                    }
                    current = current->children[folder];
                }
            }
    
            std::unordered_map<std::string, int> hashCount;
            compteHash(root, hashCount);
    
            markDuplicates(root, hashCount);
    
            std::vector<std::vector<std::string>> result;
            std::vector<std::string> currentPath;
            collectPaths(root, currentPath, result);
    
            return result;
        }
    
    private:
        std::string compteHash(FolderNode* node, std::unordered_map<std::string, int>& hashCount) {
            if (node->children.empty()) {
                return "";
            }
    
            std::string hashStr = "(";
            for (auto& [name, child] : node->children) {
                hashStr += name + compteHash(child, hashCount) + "#";
            }
            hashStr += ")";
            node->hash = hashStr;
    
            hashCount[hashStr]++;
            return hashStr;
        }
    
        void markDuplicates(FolderNode* node, std::unordered_map<std::string, int>& hashCount) {
            if (!node->children.empty() && hashCount[node->hash] > 1) {
                node->toDelete = true;
                return;
            }
    
            for (auto& [name, child] : node->children) {
                markDuplicates(child, hashCount);
            }
        }
    
        void collectPaths(FolderNode* node,  std::vector<std::string>& currentPath, std::vector<std::vector<std::string>>& result) {
            if (node->toDelete) {
                return;
            }
            if (!node->name.empty()) {
                currentPath.push_back(node->name);
                result.push_back(currentPath);
            }
            for (auto& [name, child] : node->children) {
                collectPaths(child, currentPath, result);
            }
    
            if (!node->name.empty()) {
                currentPath.pop_back();
            }
        }
    };
    ```