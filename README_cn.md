# OS Camp - Rust 与操作系统进阶实验

一个采用 [rustlings](https://github.com/rust-lang/rustlings) 风格的 Rust 进阶与操作系统入门练习仓库。学习 Rust 并发编程、异步编程，`no_std`通过完成代码和通过测试，学习 Rust 编程、操作系统开发和操作系统核心概念。

## 先决条件

- Rust 工具链（稳定版，>= 1.75）
- Linux 环境：大多数练习针对 x86_64；**模块 4（上下文切换）仅支持 riscv64**，需要 riscv64 环境或 QEMU 用户模式仿真

```bash
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

## 练习结构

**共 6 个模块，23 个练习**，从易到难：### 模块一：并发（同步）—`01_concurrency_sync/`| # | 练习 | 涉及概念 |
|---|----------|----------|| 1 |`01_thread_spawn` | `01_thread_s1 | `01_thread_`spawn` | `thread::spawn`、`move` 闭包、`join` || 2 |`02_mutex_counter` | `02_mutex_co`Arc<Mutex<T>>` | 共享状态并发 || 3 |`03_channel` | `03_channel`| `mpsc::channel`，多生产者模式 || 4 |`04_process_pipe` | `04_pr4 | `04_process_管道（`pipe`） | `Command`、`Stdio::piped()`、进程管道 |### 模块 2：no_std 开发 —`02_no_std_dev/`| # | 练习 | 涉及概念 |
|---|----------|----------|| 1 |`01_mem_primitives` | `01_m`em_primitives` | `no_std` 内存原语：memcpy、memset、memmove、strlen、strcmp || 2 |`02_bump_allocator` | `02_bump_a`allocator` | `GlobalAlloc` 特性，Bump 分配器，基于 CAS 的线程安全 || 3 |`03_free_list_allocator`| 空闲链表分配器，侵入式链表，首次适应策略 || 4 |`04_syscall_wrapper`| 跨架构系统调用ABI（x86_64/aarch64/riscv64），内联汇编 || 5 |`05_fd_table`| 文件 descriptor tab| 文件描述符（fd）重用策略 |### 模块三：操作系统并发进阶 —`03_os_concurrency/`| # | 练习 | 涉及概念 |
|---|----------|----------|| 1 |`01_atomic_counter` | `01_atom1 | `01_ato`mic_counter` | `AtomicU64`, `fetch_add`, CAS 循环 || 2 |`02_atomic_ordering`| 内存优化rdering, R释放-获取，`OnceCell` || 3 |`03_spinlock`| 旋转锁 |ck implementation,3 | `03_spi`spinlock` | 自旋锁实现，`compare_exchange`，`spin_loop` || 4 |`04_spinlock_guard`ock_gua 4 | `04_s 4 | `04_spinlock_guard` | RAII 守卫，`Deref`/`DerefMut`/`Drop` || 5 |`05_rwlock`| 写者优先读写锁from scratch (no `s`td::sync::RwLock`) |### 模块 4：上下文切换 —`04_context_switch/`（仅限 riscv64）

| # | 练习 | 涉及概念 |
|---|----------|----------|| 1 |`01_stack_coroutine`| 被调用者保存寄存器、栈帧、上下文切换 || 2 |`02_green_threads`| 绿色线程调度器，协作式调度，yield |模块 4 仅在 **riscv64** 架构上运行。请运行`./check.sh`y runs o一个 **riscv64** 架构。运行 `./check.sh` 或使用 `oscamp` 命令行界面（CLI）。as with the rest of the repository — 无需单独的脚本。详情请参阅 `exercises/04_context_switch/README.md`。### 模块 5：异步编程`05_async_programming/`| # | 练习 | 涉及概念 |
|---|----------|----------|| 1 |`01_basic_future`| 马nual imp1_basi1 | `01`_basic_future` | 手动实现 `Future` 特征、`Poll`、`Waker` || 2 |`02_tokio_tasks` | `02_tokio_t2 | `02_toki`o_tasks` | `tokio::spawn`, `JoinHandle`, 并发任务 || 3 |`03_async_channel` | `03_async_channe`l` | `tokio::sync::mpsc`, 异步生产者-消费者模式 || 4 |`04_select_timeout` | `04_select_ti`meout` | `tokio::select!`， 超时控制， 竞速执行 |### 模块 6：页表 —`06_page_table/`| # | 练习 | 概念 |
|---|----------|----------|| 1 |`01_pte_flags`| SV39 页表项（PTE）位布局，构建/解析页表项的位操作 || 2 |`02_page_table_walk`| 单级页表、虚拟页号/偏移量划分、地址转换、缺页异常 || 3 |`03_multi_level_pt`| SV39三级页表、页表遍历、大页（2MB）映射 || 4 |`04_tlb_sim`| TLB 查找/插入/FIFO 替换，刷新（全部/按页/按ASID），MMU 模拟 |

## 快速开始

```bash
# 1. 克隆仓库
git clone <repo-url> && cd oscamp-base-experiment

# 2. 构建交互式命令行工具
cargo build -p oscamp-cli

# 3. 启动交互式练习模式（推荐）
./target/debug/oscamp watch
```## 交互式命令行界面工具（Interactive CLI Tool）`oscamp`内置交互式终端工具，类似 rustlings，支持实时文件监控和进度跟踪：

```bash
oscamp              # 启动交互式监控模式（默认）
oscamp watch        # 同上
oscamp list         # 查看所有练习的完成状态
oscamp check        # 批量检查所有练习
oscamp run <pkg>    # 运行指定练习的测试
oscamp hint <pkg>   # 查看练习提示
oscamp help         # 显示帮助信息
```

### 监控模式功能

- **自动文件变更检测**：保存文件后自动重新运行测试
- **自动跳转**：当前练习通过后自动跳转到下一个未完成的练习
- **实时进度条**：显示整体完成进度
- **快捷键**：-`h`— 查看当前练习的提示-`l`— 查看所有练习列表-`n` `n`/` `p` — 下一个 / 上一个练习-`r` `r` / `Enter` 键 — 重新运行测试-`q` `q` / `Esc` 键 — 退出

### 手动执行

```bash
# 运行特定练习的测试
cargo test -p thread_spawn

# 查看详细输出
cargo test -p thread_spawn -- --nocapture

# 检查所有练习
cargo test --workspace
```

## 工作流程1. **启动**：运行`./target/debug/oscamp watch`进入交互模式2. **读取**：打开当前练习文件`src/lib.rs`，阅读文档以理解概念3. **代码**：查找`todo!()`标记，根据注释提示完成代码
4. **保存**：保存文件后，命令行界面（CLI）会自动重新运行测试5. **通过**：测试通过后，自动跳转到下一个练习；按`h`随时查看提示

## 提交分数推送至`main`将代码推送到你仓库的 `main` 分支以触发评分流水线。GitHub Actions 将自动运行所有测试，计算你的分数（满分 100 分），并将其上传到 OpenCamp 排行榜。

1.  接受 GitHub Classroom 的作业邀请链接 —— 这将创建你的个人仓库。
2.  在本地或 **GitHub Codespaces** 中完成练习（点击 "Code" > "Codespaces" > "Create codespace on main"）。3. 提交并推送您的更改到`main`4. 检查“操作”选项卡查看你的得分

## 注意事项

- 部分练习（例如，模块2的系统调用包装器、模块4的汇编）需要**Linux**环境；模块4仅支持**riscv64**架构
- 建议按模块顺序完成练习；在每个模块内，练习难度由易到难

## 许可证

MIT