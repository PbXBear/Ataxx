// #define MYDEBUG
// #define MYTEST

#include <bits/stdc++.h>
using namespace std;

#if defined(MYTEST) || defined(_BOTZONE_ONLINE)
#pragma GCC optimize(3)
#define myassert(...) ((void)0)
#else
#define myassert(...) assert(__VA_ARGS__)
#endif

// const int MAXTIME = CLOCKS_PER_SEC << 4;
const int MAXTIME = 0.96 * CLOCKS_PER_SEC;

#define FIXED 5
#ifdef FIXED
#define INITDEPTH FIXED
#else
#define INITDEPTH 1
#endif

#define VAL_MAX 100
#define VAL_MIN -100
#define INF 200
#define POS(x, y) (((x) << 3) + (y))

int mycolor = 0, rounds, cnt = 4;
int maxdepth = INITDEPTH, enddepth = 0;
uint64_t thisboard[2] = {0x40000000000001ull, 0x1000000000040ull};
clock_t start_time;
const uint64_t around[64] = {0x302, 0x705, 0xe0a, 0x1c14, 0x3828, 0x7050, 0x6020, 0, 0x30203, 0x70507, 0xe0a0e, 0x1c141c, 0x382838, 0x705070, 0x602060, 0, 0x3020300, 0x7050700, 0xe0a0e00, 0x1c141c00, 0x38283800, 0x70507000, 0x60206000, 0, 0x302030000, 0x705070000, 0xe0a0e0000, 0x1c141c0000, 0x3828380000, 0x7050700000, 0x6020600000, 0, 0x30203000000, 0x70507000000, 0xe0a0e000000, 0x1c141c000000, 0x382838000000, 0x705070000000, 0x602060000000, 0, 0x3020300000000, 0x7050700000000, 0xe0a0e00000000, 0x1c141c00000000, 0x38283800000000, 0x70507000000000, 0x60206000000000, 0, 0x2030000000000, 0x5070000000000, 0xa0e0000000000, 0x141c0000000000, 0x28380000000000, 0x50700000000000, 0x20600000000000};
const uint64_t outer_around[64] = {0x70404, 0xf0808, 0x1f1111, 0x3e2222, 0x7c4444, 0x780808, 0x701010, 0, 0x7040404, 0xf080808, 0x1f111111, 0x3e222222, 0x7c444444, 0x78080808, 0x70101010, 0, 0x704040407, 0xf0808080f, 0x1f1111111f, 0x3e2222223e, 0x7c4444447c, 0x7808080878, 0x7010101070, 0, 0x70404040700, 0xf0808080f00, 0x1f1111111f00, 0x3e2222223e00, 0x7c4444447c00, 0x780808087800, 0x701010107000, 0, 0x7040404070000, 0xf0808080f0000, 0x1f1111111f0000, 0x3e2222223e0000, 0x7c4444447c0000, 0x78080808780000, 0x70101010700000, 0, 0x4040407000000, 0x808080f000000, 0x1111111f000000, 0x2222223e000000, 0x4444447c000000, 0x8080878000000, 0x10101070000000, 0, 0x4040700000000, 0x8080f00000000, 0x11111f00000000, 0x22223e00000000, 0x44447c00000000, 0x8087800000000, 0x10107000000000};

struct move_t
{
    int color, from, to, rval;
    move_t(int c, int p1, int p2, int v) : color(c), from(p1), to(p2), rval(v) {}
    inline bool operator<(const move_t &m) const { return rval > m.rval; }
};

struct minmax_node
{
    uint64_t board[2];
    int cnt;
    minmax_node(const uint64_t b[2]) { board[0] = b[0], board[1] = b[1], cnt = __builtin_popcountll(b[0] | b[1]); }
    inline bool operator==(const minmax_node &n) const { return board[0] == n.board[0] && board[1] == n.board[1]; }
};

#define NCUT 0
#define ALPHA 1
#define BETA 2

struct minmax_result
{
    double value;
    int depth;
    int cut;
};

struct minmax_hash
{
    const uint64_t myhash[50][2] = {16221904044008824224ull, 10221858762161294109ull, 8288062064550904670ull, 12241570458921490232ull, 355316570559885957ull, 15873932318649128589ull, 10704747884604677366ull, 6349563784654468808ull, 10871599545577512730ull, 14414359179194446479ull, 11643868338066244663ull, 2332210993966807187ull, 10091246345604188545ull, 16880905050666118396ull, 17689715184143913714ull, 6218219691238923099ull, 14138041417627093291ull, 7996808486537953347ull, 15311249657966397298ull, 7965450680366663882ull, 11906478758148828850ull, 1029649375739060073ull, 15396241564193472337ull, 2223923345222485429ull, 13656642401905119475ull, 17498327489456737799ull, 12062008370023316385ull, 5158529812052336287ull, 1142905683444810545ull, 2841253524264003649ull, 182414608706582939ull, 14089542958386015863ull, 9925512094962260750ull, 14058228663476472862ull, 17142948666400993420ull, 12230105041170919533ull, 17439302567366979445ull, 5415738683315503983ull, 9001051089096452957ull, 1052092256607233047ull, 6702885717052894830ull, 6839307550311986097ull, 16961803715066412088ull, 6627361310331961600ull, 3034678071603019648ull, 2338506768793621588ull, 12885414394807681774ull, 11544049887800391652ull, 5549627994415070412ull, 3633578313878268017ull, 15267760017726203417ull, 1410699280424666777ull, 6567947403333953146ull, 8377918468862209568ull, 2594806956240575774ull, 3926036367103185451ull, 1653503778722254137ull, 11763010971672507161ull, 10675206932792812624ull, 1271144276243224798ull, 8061217825001246899ull, 11594482901918527518ull, 4677092268962915297ull, 9729314468955559722ull, 16598568217510295666ull, 2150408332415155837ull, 5745291693059961679ull, 15984697383673444853ull, 9911083249438949526ull, 18291495260793117647ull, 13950199801432863589ull, 16160721787734062826ull, 2242646669606608075ull, 17027658687861245654ull, 12812700729582045064ull, 16032611083000120736ull, 16167281685793941427ull, 140713630102433162ull, 14157448803045696278ull, 13234557933863214527ull, 16699363457396739133ull, 8556114230065752032ull, 14871106478529741789ull, 9450298166022647612ull, 1534179579630671783ull, 18427087012984366352ull, 9515755232903478737ull, 4835880395139314135ull, 827089038676250877ull, 6588170866623837584ull, 11762437951164376839ull, 4674829361758730727ull, 9802660420246287261ull, 11404960461862879387ull, 7483429164962394767ull, 1448996021814662505ull, 10997940077131675925ull, 4318967768618720616ull, 17300546638902038547ull, 17527965703695626966ull};
    inline uint64_t operator()(const minmax_node &n) const
    {
        uint64_t hash0 = myhash[n.cnt][0] ^ n.board[0];
        uint64_t hash1 = myhash[n.cnt][1] ^ n.board[1];
        return hash0 ^ hash1;
    }
};

unordered_map<minmax_node, minmax_result, minmax_hash> mem;

double min_search(const uint64_t board[2], int depth, double sup, double inf, move_t *require);
double max_search(const uint64_t board[2], int depth, double sup, double inf, move_t *require);

inline int bsearch(const int arr[], int size, int num) noexcept
{
    int left = 0, right = size, mid = size >> 1;
    while (arr[left] > num)
    {
        (arr[mid] > num ? left : right) = mid;
        mid = (left + right) >> 1;
        if (mid == left)
            return right;
    }
    return left;
}

inline void print_board(uint64_t board[2])
{
#ifdef MYDEBUG
    myassert((!((board[0] | board[1]) & 0xff80808080808080ull)));
    myassert(!(board[0] & board[1]));
    system("cls");
    for (int y = 0; y < 7; ++y)
    {
        for (int x = 0; x < 7; ++x)
        {
            int pos = POS(x, y);
            if (board[0] & 1ull << pos)
                putchar('x');
            else if (board[1] & 1ull << pos)
                putchar('o');
            else
                putchar('*');
            putchar(' ');
        }
        putchar('\n');
    }
#endif
}

inline int game_move(int color, int pos1, int pos2, uint64_t board[2])
{
    uint64_t src = 1ull << pos1;
    if (!(src & around[pos2]))
        board[color] &= ~src;
    board[color] |= 1ull << pos2;
    uint64_t change = board[!color] & around[pos2];
    board[color] |= change, board[!color] &= ~change;
}

inline bool have_legal(int color, const uint64_t board[2])
{
    uint64_t empty = ~(board[0] | board[1]) & 0x7f7f7f7f7f7f7full;
    for (int pos = 0; pos < 56; ++pos)
        if (1ull << pos & empty)
        {
            if (board[color] & around[pos])
                return true;
            else if (board[color] & outer_around[pos])
                return true;
        }
    return false;
}

inline int get_legal(int color, const uint64_t board[2], vector<move_t> &legal)
{
    static const int inner[8] = {9, 8, 7, 1, -1, -7, -8, -9};
    static const int outer[16] = {18, 17, 16, 15, 14, 10, 6, 2, -2, -6, -10, -14, -15, -16, -17, -18};
    uint64_t empty = ~(board[0] | board[1]) & 0x7f7f7f7f7f7f7full;
    int lsize = 0;
    for (int x = 0; x < 8; ++x)
        for (int y = 0; y < 8; ++y)
        {
            int pos = POS(x, y);
            if (1ull << pos & empty)
            {
                if (board[color] & around[pos])
                {
                    int end = 8 - bsearch(inner, 5, pos);
                    for (int i = bsearch(inner, 5, 55 - pos); i < end; ++i)
                    {
                        int dpos = inner[i] + pos;
                        if (1ull << dpos & board[color])
                        {
                            legal.emplace_back(color, dpos, pos, 1 + __builtin_popcountll(around[pos] & board[!color]));
                            lsize++;
                            break;
                        }
                    }
                }
                else if (board[color] & outer_around[pos])
                {
                    int end = 16 - bsearch(inner, 9, pos);
                    for (int i = bsearch(outer, 9, 55 - pos); i < end; ++i)
                    {
                        int dpos = outer[i] + pos;
                        uint64_t p = 1ull << dpos;
                        if (p & board[color] && p & outer_around[pos])
                        {
                            legal.emplace_back(color, dpos, pos, __builtin_popcountll(around[pos] & board[!color]));
                            lsize++;
                        }
                    }
                }
            }
        }
    stable_sort(legal.begin(), legal.begin() + lsize);
    return lsize;
}

inline double eval(const uint64_t board[2])
{
    static const double myeval[49] = {0.795353, 0.790000, 0.784049, 0.777447, 0.770135, 0.762054, 0.753144, 0.743344, 0.732596, 0.720845, 0.708040, 0.694137, 0.679102, 0.662913, 0.645561, 0.627055, 0.607422, 0.586711, 0.564990, 0.542351, 0.518908, 0.494794, 0.470159, 0.445170, 0.420000, 0.394830, 0.369841, 0.345206, 0.321092, 0.297649, 0.275010, 0.253289, 0.232578, 0.212945, 0.194439, 0.177087, 0.160898, 0.145863, 0.131960, 0.119155, 0.107404, 0.096656, 0.086856, 0.077946, 0.069865, 0.062553, 0.055951, 0.050000, 0.044647};
    myassert(!(board[0] & board[1]));
    myassert((!((board[0] | board[1]) & 0xff80808080808080ull)));

    int my = __builtin_popcountll(board[mycolor]), opp = __builtin_popcountll(board[!mycolor]);
    int base = my - opp, rnd = my + opp;
    uint64_t empty = ~(board[0] | board[1]) & 0x7f7f7f7f7f7f7full;
    int myact = 0, oppact = 0, mydanger = 0, oppdanger = 0;
    for (int x = 0; x < 8; ++x)
        for (int y = 0; y < 8; ++y)
        {
            int pos = POS(x, y);
            uint64_t bpos = 1ull << pos;
            if (bpos & board[mycolor])
                mydanger += __builtin_popcountll(around[pos] & empty);
            else if (bpos & board[!mycolor])
                oppdanger += __builtin_popcountll(around[pos] & empty);
            else
            {
                uint64_t field = around[pos] | outer_around[pos];
                myact += (bool)(board[mycolor] & field);
                oppact += (bool)(board[!mycolor] & field);
            }
        }
    double avgd = (double)mydanger / my - (double)oppdanger / opp;
    return base - myeval[rnd] * avgd + myeval[49 - rnd] * (myact - oppact);
}

inline double min_search(const uint64_t board[2], int depth, double sup, double inf, move_t *require)
{
    myassert(!(board[0] & board[1]));
    myassert((!((board[0] | board[1]) & 0xff80808080808080ull)));

    minmax_node now(board);
    auto it = mem.find(now);
    if (it != mem.end() && it->second.depth >= depth && !require)
    {
        switch (it->second.cut)
        {
        case ALPHA:
            if (it->second.value <= inf)
                return it->second.value;
            break;
        case BETA:
            if (it->second.value >= sup)
                return it->second.value;
            break;
        default:
            return it->second.value;
        }
        return it->second.value;
    }

    if (!depth)
    {
        double val;
        if (have_legal(!mycolor, board))
        {
            val = eval(board);
            if (it == mem.end() || it->second.depth < depth)
                mem[now] = {val, depth, NCUT};
        }
        else
        {
            val = __builtin_popcountll(board[!mycolor]) < 25 ? VAL_MAX : VAL_MIN;
            mem[now] = {val, INT_MAX, NCUT};
        }
        return val;
    }
    vector<move_t> legal;
    int size = get_legal(!mycolor, board, legal);
    if (!size)
    {
        double val = __builtin_popcountll(board[!mycolor]) < 25 ? VAL_MAX : VAL_MIN;
        mem[now] = {val, INT_MAX, NCUT};
        enddepth = enddepth > depth ? enddepth : depth;
        return val;
    }

    if (clock() - start_time >= MAXTIME)
    {
        enddepth = enddepth > depth ? enddepth : depth;
        return eval(board);
    }

    double mvalue = INF;
    int idx = 0, mycut = BETA;
    for (int i = 0; i < size; ++i)
    {
        uint64_t board_copy[2] = {board[0], board[1]};
        game_move(legal[i].color, legal[i].from, legal[i].to, board_copy);
        double value = max_search(board_copy, depth - 1, sup, inf, nullptr);
        if (value < mvalue)
        {
            mvalue = value;
            idx = i;
            if (value <= inf)
            {
                minmax_node nextb(board_copy);
                it = mem.find(nextb);
                if (it == mem.end() || it->second.depth < depth)
                    mem[nextb] = {value, depth, ALPHA};
                return mvalue;
            }
            if (value < sup)
            {
                sup = value;
                mycut = NCUT;
            }
        }
    }

    it = mem.find(now);
    if (it == mem.end() || it->second.depth < depth)
        mem[now] = {mvalue, depth, mycut};
    if (require)
        *require = legal[idx];
    return mvalue;
}

inline double max_search(const uint64_t board[2], int depth, double sup, double inf, move_t *require)
{
    myassert(!(board[0] & board[1]));
    myassert((!((board[0] | board[1]) & 0xff80808080808080ull)));

    minmax_node now(board);
    auto it = mem.find(now);
    if (it != mem.end() && it->second.depth > depth && !require)
    {
        switch (it->second.cut)
        {
        case ALPHA:
            if (it->second.value <= inf)
                return it->second.value;
            break;
        case BETA:
            if (it->second.value >= sup)
                return it->second.value;
            break;
        default:
            return it->second.value;
        }
    }

    if (!depth)
    {
        double val;
        if (have_legal(mycolor, board))
        {
            val = eval(board);
            if (it == mem.end() || it->second.depth < depth)
                mem[now] = {val, depth, NCUT};
        }
        else
        {
            val = __builtin_popcountll(board[mycolor]) > 24 ? VAL_MAX : VAL_MIN;
            mem[now] = {val, INT_MAX, NCUT};
        }
        return val;
    }

    vector<move_t> legal;
    int size = get_legal(mycolor, board, legal);
    if (!size)
    {
        double val = __builtin_popcountll(board[mycolor]) > 24 ? VAL_MAX : VAL_MIN;
        enddepth = enddepth > depth ? enddepth : depth;
        mem[now] = {val, INT_MAX, NCUT};
        return val;
    }

    if (clock() - start_time >= MAXTIME)
    {
        enddepth = enddepth > depth ? enddepth : depth;
        return eval(board);
    }

    double mvalue = -INF;
    int idx = 0, mycut = ALPHA;
    for (int i = 0; i < size; ++i)
    {
        uint64_t board_copy[2] = {board[0], board[1]};
        game_move(legal[i].color, legal[i].from, legal[i].to, board_copy);
        double value = min_search(board_copy, depth - 1, sup, inf, nullptr);
        if (value > mvalue)
        {
            mvalue = value;
            idx = i;
            if (value >= sup)
            {
                minmax_node nextb(board_copy);
                it = mem.find(nextb);
                if (it == mem.end() || it->second.depth < depth)
                    mem[nextb] = {value, depth, BETA};
                return value;
            }
            if (value > inf)
            {
                inf = value;
                mycut = NCUT;
            }
        }
    }

    it = mem.find(now);
    if (it == mem.end() || it->second.depth < depth)
        mem[now] = {mvalue, depth, mycut};
    if (require)
        *require = legal[idx];
    return mvalue;
}

inline void launcher()
{
#ifdef MYDEBUG
    print_board(thisboard);
#endif

#ifndef _BOTZONE_ONLINE
    vector<move_t> all;
    get_legal(mycolor, thisboard, all);
#endif

    move_t req = {-1, -1, -1, INT_MIN};
    double val = -INF;
    start_time = clock();

#ifdef FIXED
    maxdepth += cnt >> 3;
    val = max_search(thisboard, FIXED, INF, -INF, &req);
#else
    while (clock() - start_time < MAXTIME && maxdepth < 45)
        val = max_search(thisboard, maxdepth++, INF, -INF, &req);
#endif

    printf("%d %d %d %d\n", req.from >> 3, req.from & 7, req.to >> 3, req.to & 7);
    printf("%ld %d %d %.3f\n", clock() - start_time, maxdepth, enddepth, val);
}

int main()
{
#ifdef MYTEST
    freopen("input.txt", "r", stdin);
#endif
    srand((unsigned)time(0));
    int x1, y1, x2, y2;
    scanf("%d %d %d %d %d", &rounds, &x1, &y1, &x2, &y2);
    if (x1 >= 0)
    {
        mycolor = 1;
        game_move(0, POS(x1, y1), POS(x2, y2), thisboard);
    }
    while (rounds-- > 1)
    {
        scanf("%d %d %d %d", &x1, &y1, &x2, &y2);
        game_move(mycolor, POS(x1, y1), POS(x2, y2), thisboard);
        scanf("%d %d %d %d", &x1, &y1, &x2, &y2);
        game_move(!mycolor, POS(x1, y1), POS(x2, y2), thisboard);
    }
    cnt = __builtin_popcountll(thisboard[0] | thisboard[1]);
    launcher();
    return 0;
}
