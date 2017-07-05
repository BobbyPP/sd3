#ifndef __SOLVE_H__
#define __SOLVE_H__
#include <stdlib.h>


typedef struct {
  int nn; // numar noduri
  int **Ma; // matrice de adiacenta
} TGraphM;

typedef struct node {
  int v; // indicele nodului destinatie
  int c; // cost asociat arcului
  struct node *next;
} TNode, *ATNode;

typedef struct {
  int nn; // numar noduri
  ATNode *adl; // vector de pointeri alocati dinamici la vecini
} TGraphL;

int** make_copy_of_Ma ( TGraphM G, int*** mtrx ) {

	int i, j;

	for ( i = 0; i < G.nn; i ++ )
		for ( j = 0; j < G.nn; j ++ )
			(*mtrx)[i][j] = G.Ma[i][j];

	return *mtrx;

}

void make_zeros_on_lines ( TGraphM* G ) {

	int min, i, j;

	for ( i = 0; i < (*G).nn; i ++ ) {
		min = (*G).Ma[i][0];
		for ( j = 0; j < (*G).nn; j ++ )
			if ( min > (*G).Ma[i][j] )
				min = (*G).Ma[i][j];

		for ( j = 0; j < (*G).nn; j ++ )
			(*G).Ma[i][j] = (*G).Ma[i][j] - min;
	}

}

void make_zeros_on_columns ( TGraphM* G ) {

	int min, i, j;

	for ( j = 0; j < (*G).nn; j ++ ) {
		min = (*G).Ma[0][j];
		for ( i = 0; i < (*G).nn; i ++ )
			if ( min > (*G).Ma[i][j] )
				min = (*G).Ma[i][j];

		for ( i = 0; i < (*G).nn; i ++ )
			(*G).Ma[i][j] = (*G).Ma[i][j] - min;
	}

}

int exactly_one_zero ( int* vMa, int* vMatrix, int len ) {

	int i, nr_zeros = 0;

	for ( i = 0; i < len; i ++ )
		if ( vMa[i] == 0 && vMatrix[i] == 0 )
			nr_zeros ++;

	if ( nr_zeros != 1 )
		return -1;

	for ( i = 0; !(vMa[i] == 0 && vMatrix[i] == 0); i ++ )
		continue;

	return i;

}

void make_assignments_once ( TGraphM* G, int** l, int** c, int*** Matrix, char** marked ) {

	int i, j, ii, jj, e, *vMa = ( int* ) malloc ( (*G).nn * sizeof ( int ) ), *vMatrix = ( int* ) malloc ( (*G).nn * sizeof ( int ) );

	for ( i = 0; i < (*G).nn; i ++ )
		if ( (*l)[i] == -1 ) {
			for ( j = 0; j < (*G).nn; j ++ ) {
				vMa[j] = (*G).Ma[i][j];
				vMatrix[j] = (*Matrix)[i][j];
			}
			e = exactly_one_zero ( vMa, vMatrix, (*G).nn );
			if ( e != -1 ) {
				(*Matrix)[i][e] = 1;
				for ( ii = 0; ii < (*G).nn; ii ++ )
					if ( (*G).Ma[ii][e] == 0 && (*Matrix)[ii][e] == 0 )
						(*Matrix)[ii][e] = -1;
				(*l)[i] = e;
				(*c)[e] = i;
				(*marked)[i] = '\0';
			}
		}

	for ( j = 0; j < (*G).nn; j ++ )
		if ( (*c)[j] == -1 ) {
			for ( i = 0; i < (*G).nn; i ++ ) {
				vMa[i] = (*G).Ma[i][j];
				vMatrix[i] = (*Matrix)[i][j];
			}
			e = exactly_one_zero ( vMa, vMatrix, (*G).nn );
			if ( e != -1 ) {
				(*Matrix)[e][j] = 1;
				for ( jj = 0; jj < (*G).nn; jj ++ )
					if ( (*G).Ma[e][jj] == 0 && (*Matrix)[e][jj] == 0 )
						(*Matrix)[e][jj] = -1;
				(*c)[j] = e;
				(*l)[e] = j;
				(*marked)[e] = '\0';
			}
		}

}

int equal_arrays ( int* v1, int* v2, int len ) {

	int i;

	for ( i = 0; i < len; i ++ )
		if ( v1[i] != v2[i] )
			return 0;

	return 1;

}

void check_and_thick ( int** Matrix, int* l, int* c, char** marked, char** thick, int pos, int len ) {

	int j;

	(*marked)[pos] = 'c';
	for ( j = 0; j < len; j ++ )
		if ( Matrix[pos][j] == -1 && (*thick)[j] != 't' ) {
			(*marked)[c[j]] = 'm';
			(*thick)[j] = 't';
		}

}

int smallest_element_not_zero ( TGraphM G ) {

	int i, j, min = 0;

	for ( i = 0; i < G.nn; i ++ )
		for ( j = 0; j < G.nn; j ++ )
			min = min + G.Ma[i][j];

	for ( i = 0; i < G.nn; i ++ )
		for ( j = 0; j < G.nn; j ++ )
			if ( min > G.Ma[i][j] && G.Ma[i][j] > 0 )
				min = G.Ma[i][j];

	return min;

}

void prepare_for_next_assignment ( TGraphM* G, int*** Matrix, int** l, char** marked, char** thick ) {

	int i, j, theta;

	theta = smallest_element_not_zero ( *G );
	for ( i = 0; i < (*G).nn; i ++ )
		for ( j = 0; j < (*G).nn; j ++ )
			(*G).Ma[i][j] = (*G).Ma[i][j] - theta;

	for ( i = 0; i < (*G).nn; i ++ )
		if ( (*marked)[i] == '\0' )
			for ( j = 0; j < (*G).nn; j ++ )
				(*G).Ma[i][j] = (*G).Ma[i][j] + theta;

	for ( j = 0; j < (*G).nn; j ++ )
		if ( (*thick)[j] == 't' )
			for ( i = 0; i < (*G).nn; i ++ )
				(*G).Ma[i][j] = (*G).Ma[i][j] + theta;

	for ( i = 0; i < (*G).nn; i ++ )
		for ( j = 0; j < (*G).nn; j ++ )
			(*Matrix)[i][j] = 0;

	for ( i = 0; i < (*G).nn; i ++ ) {
		(*marked)[i] = 'm';
		(*thick)[i] = '\0';
		(*l)[i] = -1;
	}

}

int all_assignments_done ( int* c, int len ) {

	int i;

	for ( i = 0; i < len; i ++ )
		if ( c[i] == -1 )
			return 0;

	return 1;

}

int solve ( char *testInputFileName ) {

	int i, j, *l, *c, *aux_l, **Matrix, **initial_Ma, assigned = 0, sum_of_the_distances = 0;
	char *marked, *thick;
	TGraphM G;
	FILE* input_file = fopen ( testInputFileName, "r" );

	fseek ( input_file, 0, SEEK_SET );
	fscanf ( input_file, "%d", &G.nn );

	G.Ma = ( int** ) malloc ( G.nn * sizeof ( int* ) );
	for ( i = 0; i < G.nn; i ++ )
		G.Ma[i] = ( int* ) malloc ( G.nn * sizeof ( int ) );

	initial_Ma = ( int** ) malloc ( G.nn * sizeof ( int* ) );
	for ( i = 0; i < G.nn; i ++ )
		initial_Ma[i] = ( int* ) malloc ( G.nn * sizeof ( int ) );

	for ( i = 0; i < G.nn; i ++ )
		for ( j = 0; j < G.nn; j ++ )
			fscanf ( input_file, "%d", &G.Ma[i][j] );

	initial_Ma = make_copy_of_Ma ( G, &initial_Ma );

	l = ( int* ) malloc ( G.nn * sizeof ( int ) );
	c = ( int* ) malloc ( G.nn * sizeof ( int ) );
	for ( i = 0; i < G.nn; i ++ ) {
		l[i] = -1;
		c[i] = -1;
	}

	Matrix = ( int** ) malloc ( G.nn * sizeof ( int* ) );
	for ( i = 0; i < G.nn; i ++ )
		Matrix[i] = ( int* ) malloc ( G.nn * sizeof ( int ) );

	for ( i = 0; i < G.nn; i ++ )
		for ( j = 0; j < G.nn; j ++ )
			Matrix[i][j] = 0;

	make_zeros_on_lines ( &G );
	make_zeros_on_columns ( &G );

	marked = ( char* ) malloc ( G.nn * sizeof ( char ) );
	thick = ( char* ) malloc ( G.nn * sizeof ( char ) );
	for ( i = 0; i < G.nn; i ++ ) {
		marked[i] = 'm';
		thick[i] = '\0';
	}
	aux_l = ( int* ) malloc ( G.nn * sizeof ( int ) );
	make_assignments_once ( &G, &l, &c, &Matrix, &marked );

	while ( !all_assignments_done ( c, G.nn ) ) {
		if ( assigned )
			prepare_for_next_assignment ( &G, &Matrix, &l, &marked, &thick );
		for ( i = 0; i < G.nn; i ++ )
			c[i] = -1;

		make_assignments_once ( &G, &l, &c, &Matrix, &marked );
		while ( !equal_arrays ( aux_l, l, G.nn ) ) {
			for ( i = 0; i < G.nn; i ++ )
				aux_l[i] = l[i];
			make_assignments_once ( &G, &l, &c, &Matrix, &marked );
		}
		assigned = 1;

		for ( i = 0; i < G.nn; i ++ )
			if ( marked[i] == 'm' )
				check_and_thick ( Matrix, l, c, &marked, &thick, i, G.nn );

		for ( i = 0; i < G.nn; i ++ )
			aux_l[i] = -1;

	}

	for ( i = 0; i < G.nn; i ++ )
		sum_of_the_distances = sum_of_the_distances + initial_Ma[i][l[i]];

	fclose ( input_file );

	return sum_of_the_distances;

}

#endif
