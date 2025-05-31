# Managing Multiple Git Identities: A Clean Solution with Conditional Includes  

If you work on both professional and personal Git projects from the same machine, you’ve likely encountered this problem:  

**How do you maintain separate Git identities (name/email) for different projects without manually switching configurations?**  

Here’s how I solved it using Git’s built-in `includeIf` feature—with examples from my actual setup.  

---

## 🔧 The Problem  
By default, Git uses a *global* `user.name` and `user.email` (set via `git config --global`). But:  
- **Work projects** should use my corporate email (`navink@organization.com`).  
- **Personal projects** should use my personal email (`navinkumar.cloud@gmail.com`).  

Manually running `git config user.email` in each repo is error-prone.  

---

## 🎯 The Solution: Git’s `includeIf` Directive  
Git allows **conditional includes** in your global `~/.gitconfig` based on the project’s directory.  

### **My Setup**  
#### 1. Global `~/.gitconfig` (Default for most repos):  
```ini
[user]
    email = navink@organization.com
    name = Navin Kumar

# Override for personal projects in ~/sandbox/
[includeIf "gitdir:/Users/navinkumar/sandbox/"]
    path = /Users/navinkumar/sandbox/.gitconfig
```

#### 2. Personal config (`~/sandbox/.gitconfig`):  
```ini
[user]
    email = navinkumar.cloud@gmail.com
    name = Navin Kumar
```

### **How It Works**  
- Any repo inside `~/sandbox/` automatically uses the **personal email**.  
- All other repos default to the **work email**.  
- No manual commands needed—Git handles the switch!  

---

## 🛠️ How to Set This Up  
1. **Edit your global config**:  
   ```bash
   git config --global --edit
   ```
2. **Add conditional includes**:  
   ```ini
   [includeIf "gitdir:~/work/"]
       path = ~/work/.gitconfig
   [includeIf "gitdir:~/personal/"]
       path = ~/personal/.gitconfig
   ```
3. **Create per-directory configs**:  
   ```bash
   echo '[user]
   email = personal@example.com
   name = Your Name' > ~/personal/.gitconfig
   ```

---

## ✅ Verification  
Check active config for a repo:  
```bash
cd ~/sandbox/my-project
git config user.email  # Output: navinkumar.cloud@gmail.com

cd ~/work/project
git config user.email  # Output: navink@organization.com
```

---

## 💡 Why This Rocks  
- **No scripts/hooks**: Pure Git functionality.  
- **Automatic**: Just `cd` into a project—no manual setup.  
- **Flexible**: Works with SSH keys, GPG signing, etc.  

---

---

## 🔗 Further Reading  
- [Git Documentation: Conditional Includes](https://git-scm.com/docs/git-config#_conditional_includes)  
- [Managing Multiple GitHub Accounts](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/about-authentication-to-github)  

Simplify your Git workflow today! 🎉  
