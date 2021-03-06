"ast" --- 抽象语法树
********************

**源代码：** Lib/ast.py

======================================================================

"ast" 模块帮助 Python 程序处理 Python 语法的抽象语法树。抽象语法或许会
随着 Python 的更新发布而改变；该模块能够帮助理解当前语法在编程层面的样
貌。

抽象语法树可通过将 "ast.PyCF_ONLY_AST" 作为旗标传递给 "compile()" 内置
函数来生成，或是使用此模块中提供的 "parse()" 辅助函数。返回结果将是一
个对象树，，其中的类都继承自 "ast.AST"。抽象语法树可被内置的
"compile()" 函数编译为一个 Python 代码对象。


节点类
======

class ast.AST

   这是所有 AST 节点类的基类。实际上，这些节点类派生自
   "Parser/Python.asdl" 文件，其中定义的语法树示例 如下。它们在 C 语言
   模块 "_ast" 中定义，并被导出至 "ast" 模块。

   抽象语法定义的每个左侧符号(比方说，  "ast.stmt" 或者 "ast.expr")定
   义了一个类。另外，在抽象语法定义的右侧，对每一个构造器也定义了一个
   类；这些类继承自树左侧的类。比如，"ast.BinOp" 继承自 "ast.expr"。对
   于多分支产生式(也就是"和规则")，树右侧的类是抽象的；只有特定构造器
   结点的实例能被构造。

   _fields

      每个具体类都有个属性 "_fields", 用来给出所有子节点的名字。

      每个具体类的实例对它每个子节点都有一个属性，对应类型如文法中所定
      义。比如，"ast.BinOp" 的实例有个属性 "left"，类型是 "ast.expr".

      如果这些属性在文法中标记为可选（使用问号），对应值可能会是
      "None"。如果这些属性有零或多个（用星号标记），对应值会用Python的
      列表来表示。所有可能的属性必须在用 "compile()" 编译得到AST时给出
      ，且是有效的值。

   lineno
   col_offset
   end_lineno
   end_col_offset

      Instances of "ast.expr" and "ast.stmt" subclasses have "lineno",
      "col_offset", "lineno", and "col_offset" attributes.  The
      "lineno" and "end_lineno" are the first and last line numbers of
      source text span (1-indexed so the first line is line 1) and the
      "col_offset" and "end_col_offset" are the corresponding UTF-8
      byte offsets of the first and last tokens that generated the
      node. The UTF-8 offset is recorded because the parser uses UTF-8
      internally.

      Note that the end positions are not required by the compiler and
      are therefore optional. The end offset is *after* the last
      symbol, for example one can get the source segment of a one-line
      expression node using "source_line[node.col_offset :
      node.end_col_offset]".

   一个类的构造器 "ast.T" 像下面这样parse它的参数。

   * 如果有位置参数，它们必须和 "T._fields" 中的元素一样多；他们会像这
     些名字的属性一样被赋值。

   * 如果有关键字参数，它们必须被设为和给定值同名的属性。

   比方说，要创建和填充节点 "ast.UnaryOp"，你得用

      node = ast.UnaryOp()
      node.op = ast.USub()
      node.operand = ast.Constant()
      node.operand.value = 5
      node.operand.lineno = 0
      node.operand.col_offset = 0
      node.lineno = 0
      node.col_offset = 0

   或者更紧凑点

      node = ast.UnaryOp(ast.USub(), ast.Constant(5, lineno=0, col_offset=0),
                         lineno=0, col_offset=0)

在 3.8 版更改: Class "ast.Constant" is now used for all constants.

3.8 版后已移除: Old classes "ast.Num", "ast.Str", "ast.Bytes",
"ast.NameConstant" and "ast.Ellipsis" are still available, but they
will be removed in future Python releases.  In the meanwhile,
instantiating them will return an instance of a different class.


抽象文法
========

抽象文法目前定义如下

   -- ASDL's 5 builtin types are:
   -- identifier, int, string, object, constant

   module Python
   {
       mod = Module(stmt* body, type_ignore *type_ignores)
           | Interactive(stmt* body)
           | Expression(expr body)
           | FunctionType(expr* argtypes, expr returns)

           -- not really an actual node but useful in Jython's typesystem.
           | Suite(stmt* body)

       stmt = FunctionDef(identifier name, arguments args,
                          stmt* body, expr* decorator_list, expr? returns,
                          string? type_comment)
             | AsyncFunctionDef(identifier name, arguments args,
                                stmt* body, expr* decorator_list, expr? returns,
                                string? type_comment)

             | ClassDef(identifier name,
                expr* bases,
                keyword* keywords,
                stmt* body,
                expr* decorator_list)
             | Return(expr? value)

             | Delete(expr* targets)
             | Assign(expr* targets, expr value, string? type_comment)
             | AugAssign(expr target, operator op, expr value)
             -- 'simple' indicates that we annotate simple name without parens
             | AnnAssign(expr target, expr annotation, expr? value, int simple)

             -- use 'orelse' because else is a keyword in target languages
             | For(expr target, expr iter, stmt* body, stmt* orelse, string? type_comment)
             | AsyncFor(expr target, expr iter, stmt* body, stmt* orelse, string? type_comment)
             | While(expr test, stmt* body, stmt* orelse)
             | If(expr test, stmt* body, stmt* orelse)
             | With(withitem* items, stmt* body, string? type_comment)
             | AsyncWith(withitem* items, stmt* body, string? type_comment)

             | Raise(expr? exc, expr? cause)
             | Try(stmt* body, excepthandler* handlers, stmt* orelse, stmt* finalbody)
             | Assert(expr test, expr? msg)

             | Import(alias* names)
             | ImportFrom(identifier? module, alias* names, int? level)

             | Global(identifier* names)
             | Nonlocal(identifier* names)
             | Expr(expr value)
             | Pass | Break | Continue

             -- XXX Jython will be different
             -- col_offset is the byte offset in the utf8 string the parser uses
             attributes (int lineno, int col_offset, int? end_lineno, int? end_col_offset)

             -- BoolOp() can use left & right?
       expr = BoolOp(boolop op, expr* values)
            | NamedExpr(expr target, expr value)
            | BinOp(expr left, operator op, expr right)
            | UnaryOp(unaryop op, expr operand)
            | Lambda(arguments args, expr body)
            | IfExp(expr test, expr body, expr orelse)
            | Dict(expr* keys, expr* values)
            | Set(expr* elts)
            | ListComp(expr elt, comprehension* generators)
            | SetComp(expr elt, comprehension* generators)
            | DictComp(expr key, expr value, comprehension* generators)
            | GeneratorExp(expr elt, comprehension* generators)
            -- the grammar constrains where yield expressions can occur
            | Await(expr value)
            | Yield(expr? value)
            | YieldFrom(expr value)
            -- need sequences for compare to distinguish between
            -- x < 4 < 3 and (x < 4) < 3
            | Compare(expr left, cmpop* ops, expr* comparators)
            | Call(expr func, expr* args, keyword* keywords)
            | FormattedValue(expr value, int? conversion, expr? format_spec)
            | JoinedStr(expr* values)
            | Constant(constant value, string? kind)

            -- the following expression can appear in assignment context
            | Attribute(expr value, identifier attr, expr_context ctx)
            | Subscript(expr value, slice slice, expr_context ctx)
            | Starred(expr value, expr_context ctx)
            | Name(identifier id, expr_context ctx)
            | List(expr* elts, expr_context ctx)
            | Tuple(expr* elts, expr_context ctx)

             -- col_offset is the byte offset in the utf8 string the parser uses
             attributes (int lineno, int col_offset, int? end_lineno, int? end_col_offset)

       expr_context = Load | Store | Del | AugLoad | AugStore | Param

       slice = Slice(expr? lower, expr? upper, expr? step)
             | ExtSlice(slice* dims)
             | Index(expr value)

       boolop = And | Or

       operator = Add | Sub | Mult | MatMult | Div | Mod | Pow | LShift
                    | RShift | BitOr | BitXor | BitAnd | FloorDiv

       unaryop = Invert | Not | UAdd | USub

       cmpop = Eq | NotEq | Lt | LtE | Gt | GtE | Is | IsNot | In | NotIn

       comprehension = (expr target, expr iter, expr* ifs, int is_async)

       excepthandler = ExceptHandler(expr? type, identifier? name, stmt* body)
                       attributes (int lineno, int col_offset, int? end_lineno, int? end_col_offset)

       arguments = (arg* posonlyargs, arg* args, arg? vararg, arg* kwonlyargs,
                    expr* kw_defaults, arg? kwarg, expr* defaults)

       arg = (identifier arg, expr? annotation, string? type_comment)
              attributes (int lineno, int col_offset, int? end_lineno, int? end_col_offset)

       -- keyword arguments supplied to call (NULL identifier for **kwargs)
       keyword = (identifier? arg, expr value)

       -- import name with optional 'as' alias.
       alias = (identifier name, identifier? asname)

       withitem = (expr context_expr, expr? optional_vars)

       type_ignore = TypeIgnore(int lineno, string tag)
   }


"ast" 中的辅助函数
==================

除了节点类， "ast" 模块里为遍历抽象语法树定义了这些工具函数和类:

ast.parse(source, filename='<unknown>', mode='exec', *, type_comments=False, feature_version=None)

   把源码解析为AST节点。和 "compile(source, filename,
   mode,ast.PyCF_ONLY_AST)" 等价。

   If "type_comments=True" is given, the parser is modified to check
   and return type comments as specified by **PEP 484** and **PEP
   526**. This is equivalent to adding "ast.PyCF_TYPE_COMMENTS" to the
   flags passed to "compile()".  This will report syntax errors for
   misplaced type comments.  Without this flag, type comments will be
   ignored, and the "type_comment" field on selected AST nodes will
   always be "None".  In addition, the locations of "# type: ignore"
   comments will be returned as the "type_ignores" attribute of
   "Module" (otherwise it is always an empty list).

   In addition, if "mode" is "'func_type'", the input syntax is
   modified to correspond to **PEP 484** "signature type comments",
   e.g. "(str, int) -> List[str]".

   Also, setting "feature_version" to a tuple "(major, minor)" will
   attempt to parse using that Python version's grammar. Currently
   "major" must equal to "3".  For example, setting
   "feature_version=(3, 4)" will allow the use of "async" and "await"
   as variable names.  The lowest supported version is "(3, 4)"; the
   highest is "sys.version_info[0:2]".

   警告:

     足够复杂或是巨大的字符串可能导致Python解释器的崩溃，因为Python的
     AST编译器是有栈深限制的。

   在 3.8 版更改: Added "type_comments", "mode='func_type'" and
   "feature_version".

ast.literal_eval(node_or_string)

   对表达式节点以及包含Python字面量或容器的字符串进行安全的求值。传入
   的字符串或者节点里可能只包含下列的Python字面量结构: 字符串，字节对
   象(bytes)，数值，元组，列表，字典，集合，布尔值和 "None"。

   This can be used for safely evaluating strings containing Python
   values from untrusted sources without the need to parse the values
   oneself.  It is not capable of evaluating arbitrarily complex
   expressions, for example involving operators or indexing.

   警告:

     足够复杂或是巨大的字符串可能导致Python解释器的崩溃，因为Python的
     AST编译器是有栈深限制的。

   在 3.2 版更改: 目前支持字节和集合。

ast.get_docstring(node, clean=True)

   Return the docstring of the given *node* (which must be a
   "FunctionDef", "AsyncFunctionDef", "ClassDef", or "Module" node),
   or "None" if it has no docstring. If *clean* is true, clean up the
   docstring's indentation with "inspect.cleandoc()".

   在 3.5 版更改: 目前支持 "AsyncFunctionDef"

ast.get_source_segment(source, node, *, padded=False)

   Get source code segment of the *source* that generated *node*. If
   some location information ("lineno", "end_lineno", "col_offset", or
   "end_col_offset") is missing, return "None".

   If *padded* is "True", the first line of a multi-line statement
   will be padded with spaces to match its original position.

   3.8 新版功能.

ast.fix_missing_locations(node)

   When you compile a node tree with "compile()", the compiler expects
   "lineno" and "col_offset" attributes for every node that supports
   them.  This is rather tedious to fill in for generated nodes, so
   this helper adds these attributes recursively where not already
   set, by setting them to the values of the parent node.  It works
   recursively starting at *node*.

ast.increment_lineno(node, n=1)

   Increment the line number and end line number of each node in the
   tree starting at *node* by *n*. This is useful to "move code" to a
   different location in a file.

ast.copy_location(new_node, old_node)

   Copy source location ("lineno", "col_offset", "end_lineno", and
   "end_col_offset") from *old_node* to *new_node* if possible, and
   return *new_node*.

ast.iter_fields(node)

   Yield a tuple of "(fieldname, value)" for each field in
   "node._fields" that is present on *node*.

ast.iter_child_nodes(node)

   Yield all direct child nodes of *node*, that is, all fields that
   are nodes and all items of fields that are lists of nodes.

ast.walk(node)

   Recursively yield all descendant nodes in the tree starting at
   *node* (including *node* itself), in no specified order.  This is
   useful if you only want to modify nodes in place and don't care
   about the context.

class ast.NodeVisitor

   A node visitor base class that walks the abstract syntax tree and
   calls a visitor function for every node found.  This function may
   return a value which is forwarded by the "visit()" method.

   This class is meant to be subclassed, with the subclass adding
   visitor methods.

   visit(node)

      Visit a node.  The default implementation calls the method
      called "self.visit_*classname*" where *classname* is the name of
      the node class, or "generic_visit()" if that method doesn't
      exist.

   generic_visit(node)

      This visitor calls "visit()" on all children of the node.

      Note that child nodes of nodes that have a custom visitor method
      won't be visited unless the visitor calls "generic_visit()" or
      visits them itself.

   Don't use the "NodeVisitor" if you want to apply changes to nodes
   during traversal.  For this a special visitor exists
   ("NodeTransformer") that allows modifications.

   3.8 版后已移除: Methods "visit_Num()", "visit_Str()",
   "visit_Bytes()", "visit_NameConstant()" and "visit_Ellipsis()" are
   deprecated now and will not be called in future Python versions.
   Add the "visit_Constant()" method to handle all constant nodes.

class ast.NodeTransformer

   子类 "NodeVisitor"  用于遍历抽象语法树，并允许修改节点。

   "NodeTransformer" 将遍历抽象语法树并使用visitor方法的返回值去替换或
   移除旧节点。如果visitor方法的返回值为 "None" , 则该节点将从其位置移
   除，否则将替换为返回值。当返回值是原始节点时，无需替换。

   如下是一个转换器示例，它将所有出现的名称 ("foo") 重写为
   "data['foo']":

      class RewriteName(NodeTransformer):

          def visit_Name(self, node):
              return Subscript(
                  value=Name(id='data', ctx=Load()),
                  slice=Index(value=Constant(value=node.id)),
                  ctx=node.ctx
              )

   请记住，如果您正在操作的节点具有子节点，则必须先转换其子节点或为该
   节点调用 "generic_visit()" 方法。

   对于属于语句集合（适用于所有语句节点）的节点，访问者还可以返回节点
   列表而不仅仅是单个节点。

   If "NodeTransformer" introduces new nodes (that weren't part of
   original tree) without giving them location information (such as
   "lineno"), "fix_missing_locations()" should be called with the new
   sub-tree to recalculate the location information:

      tree = ast.parse('foo', mode='eval')
      new_tree = fix_missing_locations(RewriteName().visit(tree))

   通常你可以像这样使用转换器:

      node = YourTransformer().visit(node)

ast.dump(node, annotate_fields=True, include_attributes=False)

   Return a formatted dump of the tree in *node*.  This is mainly
   useful for debugging purposes.  If *annotate_fields* is true (by
   default), the returned string will show the names and the values
   for fields. If *annotate_fields* is false, the result string will
   be more compact by omitting unambiguous field names.  Attributes
   such as line numbers and column offsets are not dumped by default.
   If this is wanted, *include_attributes* can be set to true.

参见:

  Green Tree Snakes, an external documentation resource, has good
  details on working with Python ASTs.
