unsigned __int8 *__fastcall cocos2d::CCFileUtils::dataFastSet(
        cocos2d::CCFileUtils *this,
        unsigned __int8 *a2,
        unsigned int *a3,
        int a4,
        bool a5)
{
  unsigned int v7; // r1
  unsigned __int8 *result; // r0
  int v10; // r4
  unsigned int v11[7]; // [sp+14h] [bp-1Ch] BYREF

  v7 = *a3;
  result = a2;
  if ( *a3 > a4 + 4 )
  {
    result = a2;
    if ( (HIBYTE(*(_DWORD *)a2) | ((*(_DWORD *)a2 & 0xFF0000) >> 8) | ((*(_DWORD *)a2 & 0xFF00) << 8) | (*(_DWORD *)a2 << 24)) == 1146241357 )
    {
      v11[0] = 0;
      v10 = xxtea_decrypt(
              a2 + 4,
              v7 - 4,
              *((unsigned __int8 **)this + 15),
              *(_DWORD *)(*((_DWORD *)this + 15) - 12),
              v11,
              a4);
      *a3 = v11[0];
      if ( a5 )
        j_free(a2);
      return (unsigned __int8 *)v10;
    }
  }
  return result;
}


int __fastcall xxtea_decrypt(
        unsigned __int8 *a1,
        unsigned int a2,
        unsigned __int8 *a3,
        unsigned int a4,
        unsigned int *a5,
        int a6)
{
  char *v8; // r7
  int v9; // r4

  *a5 = 0;
  if ( a4 > 0xF )
    return sub_C8AFAD8((int)a1, a2, (int)a3, (int)a5, a6);
  v8 = sub_C8AF944(a3, a4);
  v9 = sub_C8AFAD8((int)a1, a2, (int)v8, (int)a5, a6);
  j_free(v8);
  return v9;
}

int __fastcall sub_C8AFAD8(int a1, int a2, int a3, int a4, int a5)
{
  unsigned int *v6; // r5
  unsigned int v7; // r4
  int v8; // r6
  unsigned int v9; // r0
  unsigned int *v10; // r12
  unsigned int *v11; // r3
  int v12; // r2
  int v13; // r6
  int v14; // r4
  unsigned int v15; // r1
  int v16; // r4
  int v18; // [sp+8h] [bp-38h]
  int v19; // [sp+Ch] [bp-34h]
  _DWORD *p; // [sp+10h] [bp-30h]
  unsigned int *v21; // [sp+18h] [bp-28h]
  unsigned int v23; // [sp+20h] [bp-20h] BYREF
  char v24[28]; // [sp+24h] [bp-1Ch] BYREF

  v6 = (unsigned int *)sub_C8AF96A(a1, a2, 0, &v23);
  p = (_DWORD *)sub_C8AF96A(a3, 16, 0, v24);
  v7 = *v6;
  v8 = v23 - 1;
  v19 = v23 - 1;
  if ( v23 != 1 )
  {
    v9 = -1640531527 * (0x34 / v23 + 6);
    v10 = &v6[v8];
    v21 = &v6[v8 - 1];
    while ( v9 )
    {
      v11 = v21;
      v12 = v19;
      do
      {
        v13 = ((v9 >> 2) ^ v12--) & 3;
        v18 = (p[v13] ^ *v11) + (v7 ^ v9);
        v14 = ((v7 >> 3) ^ (16 * *v11)) + ((*v11 >> 5) ^ (4 * v7));
        v15 = v11[1];
        --v11;
        v7 = v15 - (v14 ^ v18);
        v11[2] = v7;
      }
      while ( v12 );
      v7 = *v6 - (((v7 ^ v9) + (*v10 ^ p[(v9 >> 2) & 3])) ^ (((16 * *v10) ^ (v7 >> 3)) + ((*v10 >> 5) ^ (4 * v7))));
      *v6 = v7;
      v9 += 1640531527;
    }
  }
  v16 = sub_C8AF8D4(v6, v23, 1, a4, a5);
  j_free(v6);
  j_free(p);
  return v16;
}


char *__fastcall sub_C8AF944(const void *a1, size_t a2)
{
  char *v4; // r4

  v4 = (char *)j_malloc(0x10u);
  j_memcpy(v4, a1, a2);
  j_memset(&v4[a2], 0, 16 - a2);
  return v4;
}


char *__fastcall sub_C8AF96A(int a1, unsigned int a2, int a3, _DWORD *a4)
{
  int v6; // r5
  char *v7; // r4
  unsigned int i; // r3
  char *v9; // r2
  int v10; // r0
  int byte_count; // [sp+0h] [bp-20h]

  v6 = (a2 >> 2) + ((__PAIR64__(a2 & 3, a2 & 3) - __PAIR64__((a2 & 3) - 1, 1)) >> 32);
  byte_count = 4 * v6;
  if ( a3 )
  {
    ++v6;
    v7 = (char *)j_malloc(4 * v6);
    *(_DWORD *)&v7[byte_count] = a2;
  }
  else
  {
    v7 = (char *)j_malloc(byte_count);
  }
  *a4 = v6;
  j_memset(v7, 0, byte_count);
  for ( i = 0; i != a2; ++i )
  {
    v9 = &v7[4 * (i >> 2)];
    v10 = *(unsigned __int8 *)(a1 + i) << (8 * (i & 3));
    *(_DWORD *)v9 |= v10;
  }
  return v7;
}


_BYTE *__fastcall sub_C8AF8D4(int a1, int a2, int a3, unsigned int *a4, int a5)
{
  int v7; // r4
  int v8; // r1
  unsigned int v9; // r3
  _BYTE *result; // r0
  unsigned int v11; // r2
  unsigned int i; // r3

  v7 = 4 * a2;
  if ( a3 )
  {
    v8 = 4 * (a2 + 0x3FFFFFFF);
    v9 = *(_DWORD *)(v8 + a1);
    if ( v9 < v7 - 7 || v9 > v7 - 4 )
      return 0;
    v7 = *(_DWORD *)(v8 + a1);
  }
  result = j_malloc(v7 - a5 + 1);
  v11 = 0;
  for ( i = 0; i != v7; ++i )
  {
    if ( i >= a5 )
      result[v11++] = *(_DWORD *)(4 * (i >> 2) + a1) >> (8 * (i & 3));
  }
  if ( a5 <= 0 )
  {
    result[i] = 0;
    *a4 = i;
  }
  else
  {
    result[v11] = 0;
    *a4 = v11;
  }
  return result;
}
