package coverage

import (
	"errors"
	"os"
	"testing"
	"time"
)

// DO NOT EDIT THIS FUNCTION
func init() {
	content, err := os.ReadFile("students_test.go")
	if err != nil {
		panic(err)
	}
	err = os.WriteFile("autocode/students_test", content, 0644)
	if err != nil {
		panic(err)
	}
}

// WRITE YOUR CODE BELOW
func TestNewMatrix(t *testing.T) {
	tData := []struct {
		text     string
		Expected Matrix
		Err      error
	}{
		{text: "", Err: errors.New("strconv.Atoi: parsing \"\": invalid syntax")},
		{text: "1", Expected: Matrix{rows: 1, cols: 1, data: []int{1}}, Err: nil},
		{text: "1 2 3 4 5", Expected: Matrix{rows: 1, cols: 5, data: []int{1, 2, 3, 4, 5}}, Err: nil},
		{text: `1 4 5
2 5 6
3`, Err: errors.New("Rows need to be the same length")},
		{text: `1 4 5
2 5 6
3 7 8`, Expected: Matrix{rows: 3, cols: 3, data: []int{1, 4, 5, 2, 5, 6, 3, 7, 8}}},
		{text: "1 2 s 4 5", Err: errors.New("strconv.Atoi: parsing \"\": invalid syntax")},
		{text: "              ", Err: errors.New("strconv.Atoi: parsing \"\": invalid syntax")},
		{text: "1      2 3      4 5", Err: errors.New("too much spaces between numbers")},
		{text: `1

2


3`, Err: errors.New("too much spaces between numbers")},
	}
	for _, v := range tData {
		got, err := New(v.text)

		if err != nil && v.Err != nil {
			continue
		}
		if v.Err != nil {
			t.Errorf("expected error but did not receive: %s", v.Err.Error())
			continue
		}
		if err != nil {
			t.Errorf("error happend while not expected: %s", err.Error())
			continue
		}
		isEqu, errMessage, expectedNumber, gotNumber := isEqual(got, &v.Expected)
		if !isEqu {
			t.Errorf("%s Expected: [%d] Got: [%d]", errMessage, expectedNumber, gotNumber)
		}
	}
}

func isEqual(got, Expected *Matrix) (bool, string, int, int) {
	if got.cols != Expected.cols {
		return false, "Mismatch in columns", Expected.cols, got.cols
	}
	if got.rows != Expected.rows {
		return false, "Mismatch in rows", Expected.rows, got.rows
	}

	for k, v := range got.data {
		if v != Expected.data[k] {
			return false, "Mismatch in Data", Expected.data[k], got.data[k]
		}
	}

	return true, "None", 0, 0
}

func TestGetRows(t *testing.T) {
	tData := []struct {
		matrix   Matrix
		Expected [][]int
		Err      error
	}{
		{matrix: Matrix{rows: 1, cols: 1, data: []int{1}}, Expected: [][]int{{1}}},
		{matrix: Matrix{rows: 1, cols: 5, data: []int{1, 2, 3, 4, 5}}, Expected: [][]int{{1, 2, 3, 4, 5}}},
		{matrix: Matrix{rows: 3, cols: 3, data: []int{1, 4, 5, 2, 5, 6, 3, 7, 8}}, Expected: [][]int{{1, 4, 5}, {2, 5, 6}, {3, 7, 8}}},
		{matrix: Matrix{rows: 0, cols: 0, data: []int{}}, Expected: nil},
	}
	for _, v := range tData {
		got := v.matrix.Rows()

		isEqu, expectedNumber, gotNumber := isMatrixEqual(got, v.Expected)
		if !isEqu {
			if expectedNumber+gotNumber == 0 {
				t.Errorf("different sizes")
			} else {
				t.Errorf("Expected: [%d] Got: [%d]", expectedNumber, gotNumber)
			}
		}
	}
}

func TestGetColumns(t *testing.T) {
	tData := []struct {
		matrix   Matrix
		Expected [][]int
	}{
		{matrix: Matrix{rows: 1, cols: 1, data: []int{1}}, Expected: [][]int{{1}}},
		{matrix: Matrix{rows: 1, cols: 5, data: []int{1, 2, 3, 4, 5}}, Expected: [][]int{{1}, {2}, {3}, {4}, {5}}},
		{matrix: Matrix{rows: 3, cols: 3, data: []int{1, 4, 5, 2, 5, 6, 3, 7, 8}}, Expected: [][]int{{1, 2, 3}, {4, 5, 7}, {5, 6, 8}}},
		{matrix: Matrix{rows: 0, cols: 0, data: []int{}}, Expected: nil},
	}
	for _, v := range tData {
		got := v.matrix.Cols()

		isEqu, expectedNumber, gotNumber := isMatrixEqual(got, v.Expected)
		if !isEqu {
			if expectedNumber+gotNumber == 0 {
				t.Errorf("different sizes")
			} else {
				t.Errorf("Expected: [%d] Got: [%d]", expectedNumber, gotNumber)
			}
		}
	}
}

func isMatrixEqual(got, expected [][]int) (bool, int, int) {
	if len(got) != len(expected) {
		return false, 0, 0
	}
	for k, v := range expected {
		if len(got[k]) != len(v) {
			return false, 0, 0
		}
		for t, p := range v {
			if p != got[k][t] {
				return false, p, got[k][t]
			}
		}
	}
	return true, 0, 0
}

func TestSetElement(t *testing.T) {
	tData := []struct {
		matrix          Matrix
		row, col, value int
		isChanged       bool
	}{
		{matrix: Matrix{rows: 1, cols: 1, data: []int{1}}, row: 0, col: 0, value: 5, isChanged: true},
		{matrix: Matrix{rows: 1, cols: 1, data: []int{1}}, row: 0, col: 2, value: 5, isChanged: false},
		{matrix: Matrix{rows: 1, cols: 5, data: []int{1, 2, 3, 4, 5}}, row: 0, col: 4, value: 5, isChanged: true},
		{matrix: Matrix{rows: 1, cols: 5, data: []int{1, 2, 3, 4, 5}}, row: 1, col: 4, value: 43, isChanged: false},
		{matrix: Matrix{rows: 3, cols: 3, data: []int{1, 4, 5, 2, 5, 6, 3, 7, 8}}, row: 2, col: 2, value: 11, isChanged: true},
		{matrix: Matrix{rows: 0, cols: 0, data: []int{}}, row: 1, col: 1, value: -5, isChanged: false},
	}
	for _, v := range tData {
		got := v.matrix.Set(v.row, v.col, v.value)
		if got != v.isChanged {
			t.Errorf("You Changed/didn't change value incorretly")
		} else if got && v.matrix.data[v.matrix.rows*v.col+v.row] != v.value {
			t.Errorf("Expected: [%d] Got: [%d]", v.value, v.matrix.data[v.matrix.rows*v.col+v.row])
		}
	}
}

func TestLenPeople(t *testing.T) {
	tData := []struct {
		people People
		length int
	}{
		{people: People{Person{firstName: "Anna", lastName: "dane", birthDay: time.Date(2001, 10, 6, 0, 0, 0, 0, time.UTC)}}, length: 1},
		{people: People{
			Person{firstName: "Ban", lastName: "dane", birthDay: time.Date(2011, 3, 7, 0, 0, 0, 0, time.UTC)},
			Person{firstName: "Larry", lastName: "dane", birthDay: time.Date(2012, 4, 10, 0, 0, 0, 0, time.UTC)},
		}, length: 2},
		{people: People{
			Person{firstName: "Magic", lastName: "Johnson", birthDay: time.Date(1999, 5, 22, 0, 0, 0, 0, time.UTC)},
			Person{firstName: "Gigi", lastName: "Tsirekdze", birthDay: time.Date(1990, 1, 15, 0, 0, 0, 0, time.UTC)},
			Person{firstName: "Gega", lastName: "falavandishvili", birthDay: time.Date(2007, 9, 26, 0, 0, 0, 0, time.UTC)},
			Person{firstName: "tornike", lastName: "Gvari", birthDay: time.Date(1969, 4, 12, 0, 0, 0, 0, time.UTC)},
		}, length: 4},
	}

	for _, v := range tData {
		got := v.people.Len()
		if got != v.length {
			t.Errorf("Expected: [%d] Got: [%d]", v.length, got)
		}
	}
}

func TestLessPeople(t *testing.T) {
	tData := []struct {
		people People
		i, j   int
		isLess bool
	}{
		{people: People{Person{firstName: "Anna", lastName: "dane", birthDay: time.Date(2001, 10, 6, 0, 0, 0, 0, time.UTC)}}, i: 0, j: 0, isLess: false},
		{people: People{
			Person{firstName: "Ban", lastName: "dane", birthDay: time.Date(2011, 3, 7, 0, 0, 0, 0, time.UTC)},
			Person{firstName: "Larry", lastName: "dane", birthDay: time.Date(2012, 4, 10, 0, 0, 0, 0, time.UTC)},
		}, i: 1, j: 0, isLess: true},
		{people: People{
			Person{firstName: "Magic", lastName: "Johnson", birthDay: time.Date(1999, 5, 22, 0, 0, 0, 0, time.UTC)},
			Person{firstName: "Gigi", lastName: "Tsirekdze", birthDay: time.Date(1990, 1, 15, 0, 0, 0, 0, time.UTC)},
			Person{firstName: "Gega", lastName: "falavandishvili", birthDay: time.Date(2007, 9, 26, 0, 0, 0, 0, time.UTC)},
			Person{firstName: "tornike", lastName: "Gvari", birthDay: time.Date(1969, 4, 12, 0, 0, 0, 0, time.UTC)},
		}, i: 3, j: 1, isLess: false},
		{people: People{
			Person{firstName: "Magic", lastName: "Johnson", birthDay: time.Date(1999, 5, 22, 0, 0, 0, 0, time.UTC)},
			Person{firstName: "Gigi", lastName: "Tsirekdze", birthDay: time.Date(1990, 1, 15, 0, 0, 0, 0, time.UTC)},
			Person{firstName: "Gega", lastName: "falavandishvili", birthDay: time.Date(2007, 9, 26, 0, 0, 0, 0, time.UTC)},
			Person{firstName: "tornike", lastName: "Gvari", birthDay: time.Date(1969, 4, 12, 0, 0, 0, 0, time.UTC)},
		}, i: 2, j: 0, isLess: true},
		{people: People{
			Person{firstName: "Magic", lastName: "Johnson", birthDay: time.Date(1999, 5, 22, 0, 0, 0, 0, time.UTC)},
			Person{firstName: "Gigi", lastName: "Tsirekdze", birthDay: time.Date(1990, 1, 15, 0, 0, 0, 0, time.UTC)},
			Person{firstName: "Gega", lastName: "falavandishvili", birthDay: time.Date(2007, 9, 26, 0, 0, 0, 0, time.UTC)},
			Person{firstName: "tornike", lastName: "Gvari", birthDay: time.Date(1969, 4, 12, 0, 0, 0, 0, time.UTC)},
		}, i: 3, j: 0, isLess: false},
	}

	for _, v := range tData {
		got := v.people.Less(v.i, v.j)
		if got != v.isLess {
			if got {
				t.Errorf("Expected: false Got: true")
			} else {
				t.Errorf("Expected: true Got: false")
			}
		}
	}
}

func TestSwapPeople(t *testing.T) {
	tData := []struct {
		people               People
		i, j                 int
		ExpectedI, ExpectedJ Person
	}{
		{people: People{Person{firstName: "Anna", lastName: "dane", birthDay: time.Date(2001, 10, 6, 0, 0, 0, 0, time.UTC)}}, i: 0, j: 0},
		{people: People{
			Person{firstName: "Ban", lastName: "dane", birthDay: time.Date(2011, 3, 7, 0, 0, 0, 0, time.UTC)},
			Person{firstName: "Larry", lastName: "dane", birthDay: time.Date(2012, 4, 10, 0, 0, 0, 0, time.UTC)},
		}, i: 1, j: 0},
		{people: People{
			Person{firstName: "Magic", lastName: "Johnson", birthDay: time.Date(1999, 5, 22, 0, 0, 0, 0, time.UTC)},
			Person{firstName: "Gigi", lastName: "Tsirekdze", birthDay: time.Date(1990, 1, 15, 0, 0, 0, 0, time.UTC)},
			Person{firstName: "Gega", lastName: "falavandishvili", birthDay: time.Date(2007, 9, 26, 0, 0, 0, 0, time.UTC)},
			Person{firstName: "tornike", lastName: "Gvari", birthDay: time.Date(1969, 4, 12, 0, 0, 0, 0, time.UTC)},
		}, i: 3, j: 1},
		{people: People{
			Person{firstName: "Magic", lastName: "Johnson", birthDay: time.Date(1999, 5, 22, 0, 0, 0, 0, time.UTC)},
			Person{firstName: "Gigi", lastName: "Tsirekdze", birthDay: time.Date(1990, 1, 15, 0, 0, 0, 0, time.UTC)},
			Person{firstName: "Gega", lastName: "falavandishvili", birthDay: time.Date(2007, 9, 26, 0, 0, 0, 0, time.UTC)},
			Person{firstName: "tornike", lastName: "Gvari", birthDay: time.Date(1969, 4, 12, 0, 0, 0, 0, time.UTC)},
		}, i: 2, j: 0},
		{people: People{
			Person{firstName: "Magic", lastName: "Johnson", birthDay: time.Date(1999, 5, 22, 0, 0, 0, 0, time.UTC)},
			Person{firstName: "Gigi", lastName: "Tsirekdze", birthDay: time.Date(1990, 1, 15, 0, 0, 0, 0, time.UTC)},
			Person{firstName: "Gega", lastName: "falavandishvili", birthDay: time.Date(2007, 9, 26, 0, 0, 0, 0, time.UTC)},
			Person{firstName: "tornike", lastName: "Gvari", birthDay: time.Date(1969, 4, 12, 0, 0, 0, 0, time.UTC)},
		}, i: 3, j: 0},
	}

	for _, v := range tData {
		v.ExpectedI = v.people[v.i]
		v.ExpectedJ = v.people[v.j]
		v.people.Swap(v.i, v.j)
		if v.ExpectedJ != v.people[v.i] || v.ExpectedI != v.people[v.j] {
			t.Errorf("Numbers are not swapped")
		}
	}
}
