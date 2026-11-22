 import 'package:flutter/material.dart';
import 'package:flutter/services.dart';
import 'package:csv/csv.dart';
import 'package:mobile_scanner/mobile_scanner.dart';
import 'package:shared_preferences/shared_preferences.dart';

void main() {
  WidgetsFlutterBinding.ensureInitialized();
  runApp(const FuxApp());
}

class Product {
  final String barcode;
  final String name;
  final double price;
  const Product({required this.barcode, required this.name, required this.price});
}

class FuxApp extends StatelessWidget {
  const FuxApp({super.key});

  @override
  Widget build(BuildContext context) {
    const navy = Color(0xFF10243A);
    const teal = Color(0xFF12A889);
    return MaterialApp(
      debugShowCheckedModeBanner: false,
      title: 'FUX',
      theme: ThemeData(
        useMaterial3: true,
        colorScheme: ColorScheme.fromSeed(seedColor: teal),
        scaffoldBackgroundColor: const Color(0xFFF4F7FA),
        appBarTheme: const AppBarTheme(
          backgroundColor: navy,
          foregroundColor: Colors.white,
          centerTitle: false,
        ),
        cardTheme: CardThemeData(
          color: Colors.white,
          elevation: 1,
          shape: RoundedRectangleBorder(borderRadius: BorderRadius.circular(18)),
        ),
      ),
      home: const FuxHome(),
    );
  }
}

class FuxHome extends StatefulWidget {
  const FuxHome({super.key});
  @override
  State<FuxHome> createState() => _FuxHomeState();
}

class _FuxHomeState extends State<FuxHome> {
  List<Product> products = [];
  Map<String, int> counts = {};
  bool loading = true;
  int page = 0;
  String query = '';
  Product? selected;

  static const pages = ['الرئيسية', 'المنتجات والأسعار', 'الجرد'];

  @override
  void initState() {
    super.initState();
    _load();
  }

  Future<void> _load() async {
    try {
      final raw = await rootBundle.loadString('assets/data/price_list.csv');
      final rows = const CsvToListConverter(eol: '\n').convert(raw);
      final loaded = <Product>[];
      for (final row in rows.skip(1)) {
        if (row.length < 3) continue;
        final barcode = row[0].toString().trim();
        final name = row[1].toString().trim();
        final price = double.tryParse(row[2].toString().trim()) ?? 0;
        if (barcode.isNotEmpty && name.isNotEmpty) {
          loaded.add(Product(barcode: barcode, name: name, price: price));
        }
      }
      final prefs = await SharedPreferences.getInstance();
      final saved = prefs.getStringList('fux_counts') ?? [];
      final restored = <String, int>{};
      for (final item in saved) {
        final parts = item.split('|');
        if (parts.length == 2) restored[parts[0]] = int.tryParse(parts[1]) ?? 0;
      }
      if (mounted) setState(() {
        products = loaded;
        counts = restored;
        loading = false;
      });
    } catch (e) {
      if (mounted) setState(() => loading = false);
    }
  }

  Future<void> _saveCounts() async {
    final prefs = await SharedPreferences.getInstance();
    await prefs.setStringList('fux_counts', counts.entries.map((e) => '${e.key}|${e.value}').toList());
  }

  List<Product> get filtered {
    final q = query.trim().toLowerCase();
    if (q.isEmpty) return products;
    return products.where((p) =>
      p.name.toLowerCase().contains(q) || p.barcode.toLowerCase().contains(q)).toList();
  }

  Product? byBarcode(String code) {
    for (final p in products) {
      if (p.barcode == code) return p;
    }
    return null;
  }

  Future<void> _scan() async {
    final code = await Navigator.push<String>(
      context, MaterialPageRoute(builder: (_) => const ScanPage()));
    if (code == null || code.isEmpty || !mounted) return;
    final p = byBarcode(code);
    setState(() {
      query = code;
      selected = p;
      page = 1;
    });
    if (p == null) {
      ScaffoldMessenger.of(context).showSnackBar(
        SnackBar(content: Text('لم يتم العثور على باركود: $code')));
    }
  }

  void _openProduct(Product p) {
    showModalBottomSheet(
      context: context,
      isScrollControlled: true,
      backgroundColor: Colors.transparent,
      builder: (_) => ProductSheet(
        product: p,
        count: counts[p.barcode] ?? 0,
        onCount: (value) {
          setState(() => counts[p.barcode] = value);
          _saveCounts();
        },
      ),
    );
  }

  @override
  Widget build(BuildContext context) {
    return Directionality(
      textDirection: TextDirection.rtl,
      child: Scaffold(
        appBar: AppBar(
          title: Row(children: [
            Container(
              width: 38, height: 38,
              decoration: BoxDecoration(color: Colors.white.withValues(alpha: .13), borderRadius: BorderRadius.circular(12)),
              child: const Icon(Icons.inventory_2_rounded, color: Color(0xFF45D6B3)),
            ),
            const SizedBox(width: 10),
            const Text('FUX', style: TextStyle(fontWeight: FontWeight.w900, letterSpacing: 1.2)),
          ]),
          actions: [
            IconButton(tooltip: 'مسح الباركود', onPressed: _scan, icon: const Icon(Icons.qr_code_scanner_rounded)),
            const SizedBox(width: 6),
          ],
        ),
        drawer: _drawer(),
        body: loading
          ? const Center(child: CircularProgressIndicator())
          : _body(),
        floatingActionButton: FloatingActionButton.extended(
          onPressed: _scan,
          backgroundColor: const Color(0xFF12A889),
          foregroundColor: Colors.white,
          icon: const Icon(Icons.qr_code_scanner),
          label: const Text('مسح باركود'),
        ),
        bottomNavigationBar: NavigationBar(
          selectedIndex: page,
          onDestinationSelected: (v) => setState(() { page = v; selected = null; query = ''; }),
          destinations: const [
            NavigationDestination(icon: Icon(Icons.home_outlined), selectedIcon: Icon(Icons.home), label: 'الرئيسية'),
            NavigationDestination(icon: Icon(Icons.inventory_2_outlined), selectedIcon: Icon(Icons.inventory_2), label: 'المنتجات'),
            NavigationDestination(icon: Icon(Icons.fact_check_outlined), selectedIcon: Icon(Icons.fact_check), label: 'الجرد'),
          ],
        ),
      ),
    );
  }

  Widget _drawer() => Drawer(
    child: Directionality(
      textDirection: TextDirection.rtl,
      child: Column(children: [
        Container(
          width: double.infinity, padding: const EdgeInsets.fromLTRB(22, 54, 22, 24),
          decoration: const BoxDecoration(
            gradient: LinearGradient(colors: [Color(0xFF10243A), Color(0xFF1B4965)])),
          child: const Column(crossAxisAlignment: CrossAxisAlignment.start, children: [
            Icon(Icons.inventory_2_rounded, color: Color(0xFF45D6B3), size: 42),
            SizedBox(height: 12),
            Text('FUX', style: TextStyle(color: Colors.white, fontSize: 27, fontWeight: FontWeight.bold)),
            Text('إدارة المنتجات والجرد', style: TextStyle(color: Colors.white70)),
          ]),
        ),
        _drawerItem(Icons.home_outlined, 'الرئيسية', 0),
        _drawerItem(Icons.qr_code_scanner, 'مسح الباركود', -1),
        _drawerItem(Icons.search, 'المنتجات والأسعار', 1),
        _drawerItem(Icons.fact_check_outlined, 'الجرد', 2),
        const Spacer(),
        const Divider(),
        const ListTile(leading: Icon(Icons.info_outline), title: Text('FUX • الإصدار 1.0.0')),
      ]),
    ),
  );

  Widget _drawerItem(IconData icon, String label, int target) => ListTile(
    leading: Icon(icon, color: const Color(0xFF1B4965)),
    title: Text(label, style: const TextStyle(fontWeight: FontWeight.w600)),
    onTap: () {
      Navigator.pop(context);
      if (target == -1) { _scan(); return; }
      setState(() { page = target; selected = null; query = ''; });
    },
  );

  Widget _body() {
    if (page == 1) return _productsPage();
    if (page == 2) return _countPage();
    return _homePage();
  }

  Widget _homePage() => ListView(
    padding: const EdgeInsets.all(16),
    children: [
      Container(
        padding: const EdgeInsets.all(20),
        decoration: BoxDecoration(
          borderRadius: BorderRadius.circular(22),
          gradient: const LinearGradient(colors: [Color(0xFF10243A), Color(0xFF1B4965)]),
        ),
        child: Column(crossAxisAlignment: CrossAxisAlignment.start, children: [
          const Text('مرحباً بك في FUX', style: TextStyle(color: Colors.white, fontSize: 22, fontWeight: FontWeight.bold)),
          const SizedBox(height: 6),
          const Text('ابحث عن المنتج أو امسح الباركود للوصول السريع إلى بياناته', style: TextStyle(color: Colors.white70, height: 1.5)),
          const SizedBox(height: 18),
          SizedBox(width: double.infinity, child: FilledButton.icon(
            style: FilledButton.styleFrom(backgroundColor: const Color(0xFF12A889), foregroundColor: Colors.white, padding: const EdgeInsets.symmetric(vertical: 14)),
            onPressed: _scan, icon: const Icon(Icons.qr_code_scanner), label: const Text('ابدأ مسح الباركود'),
          )),
        ]),
      ),
      const SizedBox(height: 16),
      Row(children: [
        Expanded(child: _statCard('إجمالي المنتجات', products.length.toString(), Icons.inventory_2_outlined)),
        const SizedBox(width: 12),
        Expanded(child: _statCard('أصناف تم جردها', counts.length.toString(), Icons.fact_check_outlined)),
      ]),
      const SizedBox(height: 18),
      const Text('الوصول السريع', style: TextStyle(fontSize: 18, fontWeight: FontWeight.bold)),
      const SizedBox(height: 10),
      _actionTile(Icons.search, 'البحث عن منتج', 'ابحث بالاسم أو رقم الباركود', () => setState(() => page = 1)),
      _actionTile(Icons.price_check, 'قائمة الأسعار', 'عرض المنتجات والأسعار المسجلة', () => setState(() => page = 1)),
      _actionTile(Icons.fact_check, 'متابعة الجرد', 'عرض الكميات التي تم تسجيلها', () => setState(() => page = 2)),
    ],
  );

  Widget _statCard(String title, String value, IconData icon) => Card(
    child: Padding(padding: const EdgeInsets.all(16), child: Column(crossAxisAlignment: CrossAxisAlignment.start, children: [
      Icon(icon, color: const Color(0xFF12A889), size: 25),
      const SizedBox(height: 12),
      Text(value, style: const TextStyle(fontSize: 24, fontWeight: FontWeight.w800)),
      Text(title, style: const TextStyle(color: Colors.black54)),
    ])),
  );

  Widget _actionTile(IconData icon, String title, String subtitle, VoidCallback onTap) => Card(
    child: ListTile(
      contentPadding: const EdgeInsets.symmetric(horizontal: 16, vertical: 6),
      leading: CircleAvatar(backgroundColor: const Color(0xFFE5F6F1), child: Icon(icon, color: const Color(0xFF12866F))),
      title: Text(title, style: const TextStyle(fontWeight: FontWeight.bold)),
      subtitle: Text(subtitle),
      trailing: const Icon(Icons.chevron_left),
      onTap: onTap,
    ),
  );

  Widget _productsPage() => Column(children: [
    Padding(
      padding: const EdgeInsets.fromLTRB(16, 16, 16, 8),
      child: TextField(
        textDirection: TextDirection.rtl,
        onChanged: (v) => setState(() => query = v),
        decoration: InputDecoration(
          hintText: 'ابحث باسم المنتج أو الباركود...',
          prefixIcon: const Icon(Icons.search),
          suffixIcon: IconButton(onPressed: _scan, icon: const Icon(Icons.qr_code_scanner)),
          filled: true, fillColor: Colors.white,
          border: OutlineInputBorder(borderRadius: BorderRadius.circular(16), borderSide: BorderSide.none),
        ),
      ),
    ),
    Padding(
      padding: const EdgeInsets.symmetric(horizontal: 18, vertical: 4),
      child: Row(children: [
        Text('${filtered.length} منتج', style: const TextStyle(color: Colors.black54)),
        const Spacer(),
        const Text('السعر بالريال', style: TextStyle(color: Colors.black54, fontSize: 12)),
      ]),
    ),
    Expanded(child: filtered.isEmpty
      ? const Center(child: Text('لا توجد منتجات مطابقة للبحث'))
      : ListView.builder(
          padding: const EdgeInsets.fromLTRB(12, 4, 12, 90),
          itemCount: filtered.length,
          itemBuilder: (_, i) {
            final p = filtered[i];
            return Card(
              child: ListTile(
                onTap: () => _openProduct(p),
                leading: Container(
                  width: 42, height: 42,
                  decoration: BoxDecoration(color: const Color(0xFFEAF5F2), borderRadius: BorderRadius.circular(12)),
                  child: const Icon(Icons.inventory_2_outlined, color: Color(0xFF12866F)),
                ),
                title: Text(p.name, maxLines: 2, overflow: TextOverflow.ellipsis, style: const TextStyle(fontWeight: FontWeight.w600)),
                subtitle: Text(p.barcode, textDirection: TextDirection.ltr, textAlign: TextAlign.right),
                trailing: Text(p.price.toStringAsFixed(2), style: const TextStyle(fontWeight: FontWeight.bold, color: Color(0xFF12866F))),
              ),
            );
          },
        )),
  ]);

  Widget _countPage() {
    final items = products.where((p) => counts.containsKey(p.barcode)).toList();
    return Column(children: [
      Padding(
        padding: const EdgeInsets.all(16),
        child: Row(children: [
          Expanded(child: _statCard('أصناف مسجلة', items.length.toString(), Icons.fact_check)),
          const SizedBox(width: 12),
          Expanded(child: _statCard('إجمالي الكميات', counts.values.fold<int>(0, (a,b) => a+b).toString(), Icons.numbers)),
        ]),
      ),
      Expanded(child: items.isEmpty
        ? const Center(child: Text('لم يتم تسجيل أي كميات بعد'))
        : ListView.builder(
          padding: const EdgeInsets.fromLTRB(12, 0, 12, 90),
          itemCount: items.length,
          itemBuilder: (_, i) {
            final p = items[i];
            return Card(child: ListTile(
              title: Text(p.name, maxLines: 2, overflow: TextOverflow.ellipsis),
              subtitle: Text('باركود: ${p.barcode}'),
              trailing: Text('الكمية: ${counts[p.barcode]}', style: const TextStyle(fontWeight: FontWeight.bold)),
              onTap: () => _openProduct(p),
            ));
          },
        )),
    ]);
  }
}

class ProductSheet extends StatefulWidget {
  final Product product;
  final int count;
  final ValueChanged<int> onCount;
  const ProductSheet({super.key, required this.product, required this.count, required this.onCount});
  @override
  State<ProductSheet> createState() => _ProductSheetState();
}

class _ProductSheetState extends State<ProductSheet> {
  late int qty;
  @override
  void initState() { super.initState(); qty = widget.count; }

  @override
  Widget build(BuildContext context) => Directionality(
    textDirection: TextDirection.rtl,
    child: Container(
      padding: EdgeInsets.fromLTRB(22, 12, 22, MediaQuery.of(context).viewInsets.bottom + 24),
      decoration: const BoxDecoration(color: Colors.white, borderRadius: BorderRadius.vertical(top: Radius.circular(26))),
      child: Column(mainAxisSize: MainAxisSize.min, crossAxisAlignment: CrossAxisAlignment.start, children: [
        Center(child: Container(width: 42, height: 4, decoration: BoxDecoration(color: Colors.black12, borderRadius: BorderRadius.circular(4)))),
        const SizedBox(height: 20),
        const Text('بيانات المنتج', style: TextStyle(fontSize: 20, fontWeight: FontWeight.bold)),
        const SizedBox(height: 14),
        Text(widget.product.name, style: const TextStyle(fontSize: 17, fontWeight: FontWeight.w600)),
        const SizedBox(height: 10),
        _detail('الباركود', widget.product.barcode),
        _detail('السعر', '${widget.product.price.toStringAsFixed(2)} ريال'),
        const Divider(height: 28),
        const Text('كمية الجرد', style: TextStyle(fontWeight: FontWeight.bold)),
        Row(children: [
          IconButton.filledTonal(onPressed: () => setState(() => qty = (qty - 1).clamp(0, 999999)), icon: const Icon(Icons.remove)),
          Expanded(child: Center(child: Text('$qty', style: const TextStyle(fontSize: 24, fontWeight: FontWeight.bold)))),
          IconButton.filledTonal(onPressed: () => setState(() => qty++), icon: const Icon(Icons.add)),
        ]),
        const SizedBox(height: 12),
        SizedBox(width: double.infinity, child: FilledButton(
          onPressed: () { widget.onCount(qty); Navigator.pop(context); },
          child: const Text('حفظ الكمية'),
        )),
      ]),
    ),
  );

  Widget _detail(String label, String value) => Padding(
    padding: const EdgeInsets.symmetric(vertical: 5),
    child: Row(children: [
      SizedBox(width: 90, child: Text(label, style: const TextStyle(color: Colors.black54))),
      Expanded(child: Text(value, style: const TextStyle(fontWeight: FontWeight.w600))),
    ]),
  );
}

class ScanPage extends StatefulWidget {
  const ScanPage({super.key});
  @override
  State<ScanPage> createState() => _ScanPageState();
}

class _ScanPageState extends State<ScanPage> {
  bool handled = false;
  @override
  Widget build(BuildContext context) => Directionality(
    textDirection: TextDirection.rtl,
    child: Scaffold(
      appBar: AppBar(title: const Text('مسح الباركود')),
      body: Stack(children: [
        MobileScanner(
          onDetect: (capture) {
            if (handled) return;
            final code = capture.barcodes.firstOrNull?.rawValue;
            if (code == null || code.isEmpty) return;
            handled = true;
            Navigator.pop(context, code);
          },
        ),
        Center(child: Container(
          width: 280, height: 170,
          decoration: BoxDecoration(border: Border.all(color: const Color(0xFF12D6A4), width: 3), borderRadius: BorderRadius.circular(18)),
        )),
        const Positioned(
          bottom: 45, left: 24, right: 24,
          child: Text('وجّه الكاميرا نحو باركود المنتج', textAlign: TextAlign.center,
            style: TextStyle(color: Colors.white, fontSize: 16, fontWeight: FontWeight.bold,
              shadows: [Shadow(color: Colors.black87, blurRadius: 8)])),
        ),
      ]),
    ),
  );
}
